Ex1_3_37 Josephus问题

```Java
import edu.princeton.cs.algs4.StdIn;
import edu.princeton.cs.algs4.StdOut;
//使用由循环单链表实现的队列完成
public class Ex1_3_37_Josephus<Item> {
    private class Node{
        Item item;
        Node next;
    }
    private int size;
    private Node last;

    public boolean isEmpty(){ return size == 0; }//last.next不可用
    public int size(){ return size; }

    public void enqueue(Item item){
        Node oldLast = last;
        last = new Node();
        last.item = item;
        if(isEmpty()) last.next = last;
        else{
            last.next = oldLast.next;
            oldLast.next = last;
        }
        size++;
    }
    public Item dequeue() {
        Item item = last.next.item;
        size--;
        if (isEmpty()) last = null;
        else {
            last.next = last.next.next;
        }
        return item;
    }

    //测试用例
    public static void main(String[] args){
        Ex1_3_37_Josephus Queue = new Ex1_3_37_Josephus();
        int N = Integer.parseInt(args[0]);
        int M = Integer.parseInt(args[1]);
        for(int i = 0; i < N; i++){ Queue.enqueue(i); }
        for(int i = 0; i < N; i++){
            Ex1_3_37_Josephus.Node node = Queue.last.next;
            for(int j = 0; j < M-2; j++){
                node = node.next;
            }
            Queue.last = node;
            StdOut.println(Queue.dequeue());
        }
    }//当N=7, M=2时输出1 3 5 0 4 2 6, 6号位最后一个
}
```
---

#### 要点
1. Queue除了使用有first和last节点的链表实现,还可以使用只有last节点的循环链表实现
2. 这种实现中,isEmpty()不能使用last.next == null判断,因为dequeue()和enqueue()中last.next开始总等于null
3. 实现思路:每次需要删除规定位次的结点,只能动态改变头结点(尾节点)引用的指向
```Java
for(int i = 0; i < N; i++){
            CircleLinkList.Node node = linkList.last.next;//找出当前头结点
            for(int j = 0; j < M-2; j++){//根据规定的位次找到当前头结点下规定位次节点
                node = node.next;
            }
            linkList.last = node;//为方便删除,让被找出节点成为头节点(实际上找出尾节点,头结点自动形成)
            StdOut.println(linkList.dequeue());//删除并打印
        }
```

