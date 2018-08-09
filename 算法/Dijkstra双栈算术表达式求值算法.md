### Dijkstra双栈算术表达式求值算法

```Java
import edu.princeton.cs.algs4.StdIn;
import edu.princeton.cs.algs4.StdOut;

public class Evaluate {
    public static void main(String[] args){
        ResizingArrayStack<String> ops = new ResizingArrayStack<>();
        ResizingArrayStack<Double> vals = new ResizingArrayStack<>();

        while (!StdIn.isEmpty()){
            String s = StdIn.readString();
            if(s.equals("("));
            else if(s.equals("+")) ops.push(s);
            else if(s.equals("-")) ops.push(s);
            else if(s.equals("*")) ops.push(s);
            else if(s.equals("/")) ops.push(s);
            else if(s.equals("sqrt")) ops.push(s);
            else if(s.equals(")")){
                String op = ops.pop();
                double val = vals.pop();
                if(op.equals("+")) val += vals.pop();
                else if(op.equals("-")) val -= vals.pop();
                else if(op.equals("*")) val *= vals.pop();
                else if(op.equals("/")) val /= vals.pop();
                else if(op.equals("sqrt")) val = Math.sqrt(val);
                vals.push(val);
            }
            else {
                vals.push(Double.parseDouble(s));
            }
        }
        StdOut.println(vals.pop());
    }
}
```

#### 要点:
1. 准备两个栈:运算符栈,操作数栈
2. 忽略左括号,将操作数和运算符分别压入对应栈内
3. 遇到右括号,弹出一个运算符,弹出所需数量的操作数,并将运算符和操作数的结果压入操作数栈
4. 方法局限性:最多只能有两个操作数在括号内运算且算式必须有左括号和右括号

#### 注意事项:
所有的字符间都要以空格隔开,在控制台下StdIn.isEmpty()需要接收ctrl+z才能结束输入,在idea中同理接收ctrl+d才能结束输入

