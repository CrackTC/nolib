计算行列式
$$
D=
\begin{vmatrix}
1 &0 &2 &4 &0\\
2 &1 &3 &0 &3\\
1 &0 &2 &1 &0\\
5 &-1&-3&-2&-4\\
-2&0 &-1&2 &0
\end{vmatrix}
$$
## 解
利用$Laplace$定理计算行列式时，选择含有零元素多的行（列）展开，可以简化计算，此题应选择按第$2$，$5$两列展开，这两列中仅有一个二阶子式非零，即
$$
\begin{vmatrix*}[r]
1&3\\
-1&-4
\end{vmatrix*}
$$
于是
$$
\begin{align}
D&=
\begin{vmatrix*}[r]
1&3\\
-1&-4
\end{vmatrix*}\cdot
(-1)^{2+4+2+5}\cdot
\begin{vmatrix*}[r]
1&2&4\\
1&2&1\\
-2&-1&2
\end{vmatrix*}\\
&=
\begin{vmatrix*}[r]
1&2&4\\
1&2&1\\
-2&-1&2
\end{vmatrix*}
\xlongequal{-r_1+r_2}
\begin{vmatrix*}[r]
1&2&4\\
0&0&-3\\
-2&-1&2
\end{vmatrix*}
=3
\begin{vmatrix*}[r]
1&2\\
-2&-1
\end{vmatrix*}\\
&=9
\end{align}
$$
#解题技巧 #线性代数