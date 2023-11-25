
# 定义
可以理解为变力沿曲线所做的功

其中，变力为

$$
\textit{\textbf{F}}(x, y) = P(x, y)\textit{\textbf{i}} + Q(x, y)\textit{\textbf{j}}
$$

与位移矢量微元$\textit{\textbf{dr}} = \mathrm{d}x\textit{\textbf{i}} + \mathrm{d}y\textit{\textbf{j}}$的点积

$$
\textit{\textbf{F}}(x, y) \cdot \textit{\textbf{dr}} = P(x, y)\mathrm{d}x + Q(x, y)\mathrm{d}y
$$

取积分即可得到

$$
\int\limits_{L} \textit{\textbf{F}}(x, y) \cdot \textit{\textbf{dr}} = \int\limits_{L} P(x, y)\mathrm{d}x + Q(x, y)\mathrm{d}y
$$

其中$P(x,y),Q(x,y)$叫做**被积函数**，$L$叫做**积分曲线**或**积分路径**

当积分曲线为闭合曲线时，可以写成

$$
\oint\limits_{L} \textit{\textbf{F}}(x, y) \cdot \textit{\textbf{dr}} = \oint\limits_{L} P(x, y)\mathrm{d}x + Q(x, y)\mathrm{d}y
$$

# 性质

1. 设$L$是有向光滑曲线，$-L$是$L$的反向曲线，则
   $$
   \int\limits_{-L} \textit{\textbf{F}}(x, y) \cdot \textit{\textbf{dr}} = -\int\limits_{L} \textit{\textbf{F}}(x, y) \cdot \textit{\textbf{dr}}
   $$

1. 如果$L_1$和$L_2$是有向光滑曲线，且$L_1$的终点与$L_2$的起点重合，则
   $$
   \int\limits_{L_1 + L_2} \textit{\textbf{F}}(x, y) \cdot \textit{\textbf{dr}} = \int\limits_{L_1} \textit{\textbf{F}}(x, y) \cdot \textit{\textbf{dr}} + \int\limits_{L_2} \textit{\textbf{F}}(x, y) \cdot \textit{\textbf{dr}}
   $$
   
# 转换
$$
P\mathrm{d}x + Q\mathrm{d}y + R\mathrm{d}z = Pcos\alpha ds + Qcos\beta ds + Rcos\gamma ds = (Pcos\alpha + Qcos\beta + Rcos\gamma)ds
$$

其中，$\alpha, \beta, \gamma$分别为$x, y, z$轴与曲线的夹角

于是$(cos\alpha, cos\beta, cos\gamma)$就是曲线的单位切向量

另一个角度看也可以理解成$\textit{\textbf{dr}}=ds\textit{\textbf{T}}$，其中$\textit{\textbf{T}}$为单位切向量

于是便完成了第二型曲线积分到第一型曲线积分的转换
