
# 定义

可以理解成一种求曲线质量的积分

形如

$$\int_Lf(x,y)ds$$

也就是每段曲线微元的密度乘上微元长度，然后求和，再取极限

# 计算

## 直角坐标系

一般给定直角坐标系下曲线方程和$f(x,y)$，求第一型曲线积分

要求的便是$ds$，也就是微元长度

显然，$ds=\sqrt{dx^2+dy^2}$

于是只需要算

$$\int_Lf(x,y)\sqrt{dx^2+dy^2}$$

就好啦

## 极坐标系

一般给定极坐标系下曲线方程和$f(x,y)$，求第一型曲线积分

将直角坐标系的$x,y$换成$r(\theta)cos\theta,r(\theta)sin\theta$

于是

$$dx=r'(\theta)cos\theta-r(\theta)sin\theta$$

$$dy=r'(\theta)sin\theta+r(\theta)cos\theta$$

$$ds=\sqrt{dx^2+dy^2}=\sqrt{r'^2(\theta)+r^2(\theta)}d\theta$$

$$\int_Lf(x,y)ds=\int_\alpha^\beta f(r(\theta)cos\theta,r(\theta)sin\theta)\sqrt{r'^2(\theta)+r^2(\theta)}d\theta$$
