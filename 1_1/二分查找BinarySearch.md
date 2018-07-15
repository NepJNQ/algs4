> ##### 要求：程序接受一个白名单文件（一列整数）作为参数，过滤名单中存在的条目，将其余条目打印
#### 1.寻常实现（利用书中管道和重定向接收输入）
```Java
public class BinarySearch{
    public static int rank(int key, int[] a){
        int low = 0;
        int high = a.length - 1;
        while(low <= high){
            int mid = (low + high)/2;
            if(a[mid] < key) low = mid + 1;
            else if(a[mid] > key) high = mid - 1;
            else return a[mid];
        }
        return -1;//无键值，返回-1
    }
    
pubilc static void main(String[] args){
    int[] whitelist = In.readInts(args[0]);
    Arrays.sort(whitelist);
    while(!StdIn.isEmpty()){
            int key = StdIn.readInt();
            if(rank(key, whitelist) < 0)
                StdOut.println(key);
    }
}
```
#### 要点：
> ##### 1.high = a.length - 1；
> ##### 2.low <= high:&ensp;当a[] = {1,3,5},查找5时,当条件只是low < high,无法找到5；
> ##### 3.mid ± 1：&ensp;若没有±1操作，当无键值时，无法有low <= high跳出循环

---
#### 2.递归实现
```Java
public int rank(int low, int high, int key, int[] a){
    int mid = (low + high)/2;
    while(low <= high){
        if(a[mid] == key) return a[mid];
        else if(a[mid] < key) return rank(mid+1, high, key, a);
        else return rank(low, mid-1, key, a);
    }
    return -1;
}
```
---
#### 3.运行注意
> 在存放java文件的文件夹中运行PowerShell，输入cmd命令后执行java BinarySearch tinyW.txt < tinyT.txt,此时tinyW.txt即参数args[0],tinyT.txt中的数由StdIn读取





