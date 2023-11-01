
# 定义
可以理解成一种求曲面质量的积分

形如

$$\iint\limits_{\Sigma} f(x,y,z)dS$$

也就是每个曲面微元的密度乘上微元面积，然后求和，再取极限

# 计算

给定曲面$\Sigma$，曲面方程为$z=f(x,y)$

只要求出面积微元$dS$，就可以计算出曲面积分

$$dS = \sqrt{1+f_x^2+f_y^2}dxdy$$

上面的公式可以成相对原来垂直于$z$轴的面积微元$dxdy$的比例，在$x$轴方向和$y$轴方向上根据斜率缩放，且相互独立

于是第一型曲面积分可以写成

$$\iint\limits_{\Sigma} f(x,y,z)dS = \iint\limits_{D} f(x,y,f(x,y))\sqrt{1+f_x^2+f_y^2}dxdy$$

其中$D$是曲面$\Sigma$在$xOy$平面上的投影

对于极坐标系，根据具体情况换元即可