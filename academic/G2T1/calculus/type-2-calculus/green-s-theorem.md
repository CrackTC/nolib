
# 定义

$$
\oint_{L^+}(P\mathrm{d}x+Q\mathrm{d}y)=\iint\limits_D\left(\frac{\partial Q}{\partial x}-\frac{\partial P}{\partial y}\right)\mathrm{d}x\mathrm{d}y
$$

其中 $L^+$ 是逆时针方向的闭曲线，$D$ 是 $L^+$ 围成的区域。

# 推导

基于一个事实：

任意路径边界上的功，等于路径围成的区域内的所有面积微元边界上的功之和。

可以这么理解：若面积微元边界与大的路径边界重合，那么这个面积微元边界上的功就是路径边界上的功；若面积微元边界与大的路径边界不重合，必然会被另一面积微元边界上所做的功因为方向相反抵消。

![[green_00.png]]

## 公式推导

![[green_01.png]]

$$
\begin{aligned}
\oint_{L^+}(P\mathrm{d}x+Q\mathrm{d}y)&=\int_{L_1^+}P\mathrm{d}x+\int_{L_2^+}Q\mathrm{d}y+\int_{L_3^+}P\mathrm{d}x+\int_{L_4^+}Q\mathrm{d}y\\
&=\int_{a_0}^{a_1}P(x,b_0)\mathrm{d}x+\int_{b_0}^{b_1}Q(a_1,y)\mathrm{d}y+\int_{a_1}^{a_0}P(x,b_1)\mathrm{d}x+\int_{b_1}^{b_0}Q(a_0,y)\mathrm{d}y\\
&=\int_{b_0}^{b_1}[Q(a_1,y)-Q(a_0,y)]\mathrm{d}y+\int_{a_0}^{a_1}[P(x,b_0)-P(x,b_1)]\mathrm{d}x\\
&=\int_{b_0}^{b_1}\int_{a_0}^{a_1}\frac{\partial Q}{\partial x}\mathrm{d}x\mathrm{d}y-\int_{a_0}^{a_1}\int_{b_0}^{b_1}\frac{\partial P}{\partial y}\mathrm{d}y\mathrm{d}x\\
&=\int_{b_0}^{b_1}\int_{a_0}^{a_1}\left(\frac{\partial Q}{\partial x}-\frac{\partial P}{\partial y}\right)\mathrm{d}x\mathrm{d}y\\
&=\iint\limits_D\left(\frac{\partial Q}{\partial x}-\frac{\partial P}{\partial y}\right)\mathrm{d}x\mathrm{d}y
\end{aligned}
$$
