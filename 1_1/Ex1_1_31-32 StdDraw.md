> ##### Ex1_1_31要求:接收整数N和double p,在圆上画出大小0.05且间距相等的N个点,每对点以概率p相连

#### 关键算法: 得到圆上等距的N个点,x=r*conα,y=r*sinα

```Java 
for(int a = 0; a < N; a++){
            point[a][0] = Math.cos(a*2*Math.PI/N);
            point[a][1] = Math.sin(a*2*Math.PI/N);
            StdDraw.point(point[a][0],point[a][1]);

        }
```
---
> ##### Ex1_1_32要求:接收整数N和double l,r,将(l,r)分为N段,画出输入流中值在每段的直方图

#### 关键算法:统计每段的输入值个数

```Java
while (!StdIn.isEmpty()){
            double input = StdIn.readDouble();
            for(int i = 0; i< N;i ++){
                if(input <= l+(i+1)*d && input > l+i*d){
                    amount[i] += 1.0/N;
                }
                double x = 1.0*i/N;
                double y = amount[i]/2.0;
                StdDraw.filledRectangle(x,y,0.5/N,y);
            }
        }
```
---
#### 关于StdDraw.filledRectangle
> ##### 默认在长宽各为1的创口绘图,参数前两个为每个直方的中心点坐标,后两个为宽度/2,高度/2