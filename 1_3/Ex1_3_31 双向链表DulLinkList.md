> ##### Ex1_3_31 双向链表DulLinkList

```Java
import edu.princeton.cs.algs4.StdIn;
import edu.princeton.cs.algs4.StdOut;

public class DulLinkList<Item> {
    private class DulNode {
        Item item;
        DulNode left;
        DulNode right;
    }

    private DulNode head;
    private DulNode tail;

    private int size;


    public int getSize() {
        return size;
    }

    public boolean isEmpty() {
        return head == null;
    }

    //从表头插入节点
    public void headInsert(Item item) {
        DulNode current = head;
        head = new DulNode();
        head.item = item;
        if (isEmpty()) {
            tail = head;
        }//链表为空时head,tail指向一个节点
        else {
            head.right = current;
        }
        size++;
    }
    //从表尾插入节点
    public void tailInsert(Item item) {
        DulNode current = tail;
        tail = new DulNode();
        tail.item = item;
        tail.left = current;
        if (isEmpty()) {
            head = tail;//tail = head错误,两个永远为null
        }//链表为空时head,tail指向一个节点
        else {
            current.right = tail;
        }
        size++;
    }
    //从表头删除节点
    public Item headDel() {
        Item item = head.item;
        head = head.right;
        size--;
        return item;
    }
    //从表尾删除节点
    public Item tailDel() {
        Item item = tail.item;
        tail = tail.left;
        size--;
        return item;
    }
    //在指定节点前插入节点
    public void beforeInsert(DulNode exist, Item item) {
        DulNode dulNode = new DulNode();
        dulNode.item = item;
        if(exist.left == null) headInsert(item);
        else{
            exist.left.right = dulNode;
            dulNode.left = exist.left;
            exist.left = dulNode;
            dulNode.right = exist;
            size++;
        }
    }
    //在指定节点后插入节点
    public void afterInsert(DulNode exist, Item item){
        DulNode dulNode = new DulNode();
        dulNode.item = item;
        if(exist.right == null) tailInsert(item);
        else{
            exist.right.left = dulNode;
            dulNode.right = exist.right;
            exist.right = dulNode;
            dulNode.left = exist;
            size++;
        }
    }
    //删除指定节点
    public Item del(DulNode dulNode){
        Item item = dulNode.item;
        if (dulNode == head) headDel();
        else if(dulNode == tail) tailDel();
        else{
            dulNode.left.right = dulNode.right;
            dulNode.right.left = dulNode.left;
        }
        return item;
    }

    //测试用例
    public static void main(String[] args){
        DulLinkList linkList = new DulLinkList();

        while(!StdIn.isEmpty()){
            String s = StdIn.readString();
            linkList.tailInsert(s);
        }
        linkList.beforeInsert(linkList.head,-1);
        StdOut.println(linkList.size + " elements");
        StdOut.println(linkList.head.item);
        StdOut.println(linkList.tail.item);
    }
}
```

---

#### 要点:
1. 插入节点时,当链表为空,head,tail都指向新节点,注意
```Java
//实例化表头
 DulNode current = head;
        head = new DulNode();
        head.item = item;
        if (isEmpty()) {
            tail = head;
        }//链表为空时head,tail指向一个节点
        else {
            head.right = current;
        }
//实例化表尾
DulNode current = tail;
        tail = new DulNode();
        tail.item = item;
        tail.left = current;
        if (isEmpty()) {
            head = tail;//tail = head错误,两个永远为null
        }//链表为空时head,tail指向一个节点
        else {
            current.right = tail;
        }
```
将实例化的节点赋值给空引用,才能让tail与head都指向实例化节点,若赋值顺序颠倒,两节点用为null
2. 注意在指定节点前/后插入节点,要判断指定节点是否是头/尾结点,否则会产生空指针错误
3. 插入节点时连接顺序不同
```Java
//前插
if(exist.left == null) headInsert(item);
        else{
            exist.left.right = dulNode;
            dulNode.left = exist.left;
            exist.left = dulNode;
            dulNode.right = exist;
            size++;
        }
//后插
if(exist.right == null) tailInsert(item);
        else{
            exist.right.left = dulNode;
            dulNode.right = exist.right;
            exist.right = dulNode;
            dulNode.left = exist;
            size++;
        }
```
在指定节点前插入节点时:在AB中指定B插入C,则先连接A.right和B.left再连接BC</br>
在指定节点后插入节点时:在AB中指定A插入C,则先连接C.right和B.left再连接AC</br>
