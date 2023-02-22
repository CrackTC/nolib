设$\alpha_1,\alpha_2,\cdots,\alpha_s$为$n$维向量组，$k_1,k_2,\cdots,k_s$为一组数，则称
$$
k_1\alpha_1+k_2\alpha_2+\cdots+k_s\alpha_s
$$
为向量组$\alpha_1,\alpha_2,\cdots,\alpha_s$的一个**线性组合**，$k_1,k_2,\cdots,k_s$称为这个线性组合的系数

对于向量组$\alpha_1,\alpha_2,\cdots,\alpha_s$，和向量$\alpha$，如果存在一组数$\lambda_1,\lambda_2,\cdots,\lambda_s$，使
$$
\lambda_1\alpha_1+\lambda_2\alpha_2+\cdots+\lambda_s\alpha_s=\alpha
$$
即向量$\alpha$可以表示为向量组$\alpha_1,\alpha_2,\cdots,\alpha_s$的线性组合，则称向量$\alpha$能由向量组$\alpha_1,\alpha_2,\cdots,\alpha_s$**线性表示**

由此可得，向量$\alpha$能由向量组$\alpha_1,\alpha_2,\cdots,\alpha_s$线性表示的充分必要条件是线性方程组$x_1\alpha_1+x_2\alpha_2+\cdots+x_s\alpha_s$有解

显然，任意一个$n$维向量
$$
\alpha=
\begin{bmatrix}
a_1\\
a_2\\
\vdots\\
a_n
\end{bmatrix}
$$
都可以由$n$个基本向量组$e_1,e_2,\cdots,e_n$线性表示，即
$$
\alpha=a_1e_1+a_2e_2+\cdots+a_ne_n
$$