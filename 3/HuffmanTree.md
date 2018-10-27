#### HuffmanTree 霍夫曼编码树
**Huffman编码**:为字母分配代码,代码长度取决于字母的相对使用频率或权重</br>
**Huffman树定义**:Hf树每个叶子结点对应一个字母,叶子结点的权重就是对应字母的出现频率,它是一颗**满树**</br>
**意义**:按照最小外部路径权重建立一棵树.一个叶结点的加权路径长度(WPL)定义为**权重乘以深度**.具有最小外部路径权重的二叉树就是,对于给定叶结点集合,WPL之和最小的二叉树,权重大的叶结点深度小,权重小的叶结点深度大.

---

#### 实现
建立n个结点的Hf树.首先创建n个初始的Hf树,每个树只包含单一叶结点,然后取出其中权重最小的两棵树合成一个树,两个叶结点作为新结点的子结点,新结点的权重即为子结点权重和<br>
本例使用**基于堆的优先队列创建Hf树**

#### 类结构: HuffNode->HuffTree->Heap

#### Heap类(最小堆)
```Java
import edu.princeton.cs.algs4.StdOut;

public class Heap<Key extends Comparable<Key>> {
    private int n;//实际大小
    private int maxsize;//最大
    private Key[] heap;

    public Heap(Key[] heap, int n, int maxsize){
        this.n = n;
        this.maxsize = maxsize;
        this.heap = heap;
    }

    public int size(){ return n;}
    private boolean isleaf(int pos){ return pos >= n/2 && pos < n;}
    private int lc(int pos){ return 2*pos+1;}
    private int rc(int pos){ return 2*pos+2;}
    private int parent(int pos){return (pos-1)/2 ;}

    //构造堆有序
    public void buildHeap(){
        for (int i = n/2-1; i >= 0; i--){
            sink(i);
        }
    }

    private boolean less(int i, int j){ return heap[i].compareTo(heap[j])<0 ;}
    private void swap(int i, int j){
        Key temp = heap[i];
        heap[i] = heap[j];
        heap[j] = temp;
    }
    private void swim(int pos){
        while (pos > 1 && less(pos, parent(pos))){
            swap(pos, parent(pos));
            pos = parent(pos);
        }
    }

    private void sink(int pos){
        while (!isleaf(pos)){
            int j = lc(pos), rc = rc(pos);
            if (less(rc, j) && rc < n) j = rc;
            if (less(pos, j)) return;
            swap(j, pos);
            pos = j;
        }
    }

    public void insert(Key k){
        if (size() == maxsize){
            StdOut.println("the heap is full");
            return;
        }
        int curr = n++;
        heap[curr] = k;
        swim(curr);
    }

    public Key removefirst(){
        Key first = heap[0];
        swap(0, --n);
        if(n != 0){
            sink(0);
            heap[n] = null;
        }
        //若heap[0] = null,最后的hufftree也被清空
        return first;
    }
}
```
+ 和algs4中Heap算法不同之处在于此处数组无空头元素,所以在索引和removefirst()等方法有边界变化;且此处buildHeap()方法针对数组直接构造堆有序

#### HuffNode(abstract)类
```Java
public abstract class HuffNode<T> {
    public abstract int weight();
    public abstract boolean isLeaf();
}
```
#### LeafNode叶子结点类
```Java
public class LeafNode<T> extends HuffNode<T> {
    private  T word;//字符
    private int weight;//频度

    public LeafNode(T word, int weight){
        this.word = word;
        this.weight = weight;
    }

    @Override
    public int weight(){ return weight;}
    @Override
    public boolean isLeaf(){return true;}
}
```
#### IntlNode内部结点类
```Java
public class IntlNode extends HuffNode {
    private int weight;
    private HuffNode lc;
    private HuffNode rc;

    public IntlNode(HuffNode lc, HuffNode rc){
        weight = lc.weight()+rc.weight();
        this.lc = lc;
        this.rc = rc;
    }

    @Override
    public boolean isLeaf(){ return false;}
    @Override
    public int weight(){ return weight;}
    public HuffNode lc(){ return lc;}
    public void setLc(HuffNode lc){this.lc = lc; }
    public HuffNode rc(){ return rc;}
    public void setRc(HuffNode rc){this.rc = rc;}
}
```

#### HuffTree类
```Java
public class HuffTree<T> implements Comparable<HuffTree<T>>{
    private HuffNode<T> root;

    //内部结点Hf树
    public HuffTree(HuffTree<T> lc, HuffTree<T> rc){
        root = new IntlNode(lc.root(), rc.root());
    }
    //叶结点Hf树
    public HuffTree(T word, int wgt){
        root = new LeafNode<T>(word, wgt);
    }

    public int weight(){ return root.weight();}
    public HuffNode root(){ return root;}

    //实现内部比较方法
    @Override
    public int compareTo(HuffTree<T> another){
        if (root.weight() < another.weight()) return -1;
        else if (root.weight() > another.weight()) return  1;
        else return 0;
    }
}
```
+ implements Comparables接口,实现CompareTo()方法,若泛型HuffTree<T>,则compareTo中参数也必须为HuffTree<T>
+ 实际上比较的就是HuffTree的权重和,所以HuffTree类实现接口和compareTo()方法
+ [Comparator和Comparable之间的区别](https://blog.csdn.net/u014133299/article/details/78608454)

#### test类和静态buildHuff()
```Java
import edu.princeton.cs.algs4.StdIn;

public class test<T> {
    public static void main(String[] args){
        HuffTree treeArray[] = new HuffTree[100];
        int i = 0;
        while (! StdIn.isEmpty()){
            treeArray[i++] = new HuffTree(StdIn.readChar(), StdIn.readInt());
            StdIn.readChar();
        }
        buildHuff(treeArray,i);
    }

    public static HuffTree buildHuff(HuffTree [] treeArray, int count){
        Heap forest = new Heap(treeArray, count, count);
        forest.buildHeap();
        HuffTree temp1, temp2, temp3 = null;
        while (forest.size() > 1){
            temp1 = (HuffTree) forest.removefirst();
            temp2 = (HuffTree) forest.removefirst();
            temp3 = new HuffTree(temp1, temp2);

            forest.insert(temp3);
        }
        return temp3;
    }
}
```
+ 第一个readChar()读入word,第二个用readInt()读入weight则中间的空格会被忽略,需要额外一次readChar()读取两个word组中间的空格或回车





