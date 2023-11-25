
# 定义

设平面区域 $G$ 内任意两点 $M_1,M_2$，$L$ 是 $G$ 内从 $M_1$ 到 $M_2$ 的分段光滑曲线，如果曲线积分

$$
\int_L P\mathrm{d}x+Q\mathrm{d}y
$$

只和 $M_1,M_2$ 的位置有关，而与 $L$ 的形状无关，则称该曲线积分在 $G$ 内与路径无关，否则称该曲线积分在 $G$ 内与路径有关。

# 充要条件

1. $$
   \oint_L P(x,y)\mathrm{d}x+Q(x,y)\mathrm{d}y=0
   $$
1. $P(x,y)\mathrm{d}x+Q(x,y)\mathrm{d}y$是 $G$ 内的某个函数 $u(x,y)$ 的全微分，即存在 $u(x,y)$，使得

   $$
   \mathrm{d}u(x,y)=P(x,y)\mathrm{d}x+Q(x,y)\mathrm{d}y
   $$
1. 在 $G$ 内恒有
   $$\frac{\partial P}{\partial y}=\frac{\partial Q}{\partial x}$$

对于第三个条件，可以理解为随便一个面积微元绕一圈，曲线积分为0，也就是做功均为0。
