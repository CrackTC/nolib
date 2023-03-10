设$A,B$为$n$阶方阵，$\lambda$为常数，$m$为正整数，则
1. $|\lambda A|=\lambda^n|A|$
2. $|AB|=|A||B|$
3. $|A^m|=|A|^m$

# 证明
（1）显然成立（根据[[det_mul|行列式倍乘]]的原理）

（3）是（2）的特例，因此仅证明（2）

设$A=(a_{ij})$，$B=(b_{ij})$，记$2n$阶行列式
$$
D=
\begin{vmatrix}
a_{11}&\cdots&a_{1n}&0&\cdots&0\\
\vdots&&\vdots&\vdots&&\vdots\\
a_{n1}&\cdots&a_{nn}&0&\cdots&0\\
-1&&&b_{11}&\cdots&b_{1n}\\
&\ddots&&\vdots&&\vdots\\
&&-1&b_{n1}&\cdots&b_{nn}
\end{vmatrix}=
\begin{vmatrix}
A&O\\
-E&B
\end{vmatrix}
$$
由[[det_val|行列式的值]]的求法可知，$D=|A||B|$

用$b_{1j}$乘第$1$列，$b_{2j}$乘第$2$列，$\cdots$，$b_{nj}$乘第$n$列，都加到第$n+j$列上$(j=1,2,\cdots,n)$（这里采取3B1B大佬的关于矩阵乘法解释能很轻松地理解qwq），有
$$
D=
\begin{vmatrix}
A&C\\
-E&O
\end{vmatrix}
$$
显然$C=AB$
$$
\begin{align}
D
&=
\begin{vmatrix}
A&C\\
-E&O
\end{vmatrix}\\
&=
(-1)^n
\begin{vmatrix}
-E&O\\
A&C
\end{vmatrix}\\
&=(-1)^{2n}
\begin{vmatrix}
E&O\\
A&C
\end{vmatrix}\\
&=
\begin{vmatrix}
E&O\\
A&C
\end{vmatrix}\\
&=
|E||C|\\
&=
|C|\\
&=|AB|
\end{align}
$$
因此$|AB|=|A||B|$

# 推论
$$
|AB|=|A||B|=|B||A|=|BA|
$$

值得注意的是，一般$|A+B|\ne|A|+|B|$