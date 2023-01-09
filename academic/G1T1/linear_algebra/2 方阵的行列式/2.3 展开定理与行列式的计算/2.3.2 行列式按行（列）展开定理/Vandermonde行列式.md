$$
D_n=
\begin{vmatrix}
1&1&\cdots&1\\
x_1&x_2&\cdots&x_n\\
x_1^2&x_2^2&\cdots&x_n^2\\
\vdots&\vdots&&\vdots\\
x_1^{n-1}&x_2^{n-1}&\cdots&x_n^{n-1}
\end{vmatrix}
=
\prod_{1\le i\lt j\le n}(x_j-x_i)
$$
# 证明
对$Vandermonde$[[行列式]]的阶数$n$应用归纳法。

当$n=2$时
$$
D_2=
\begin{vmatrix}
1&1\\
x_1&x_2
\end{vmatrix}=
(x_2-x_1)=
\prod_{1\le i\lt j\le 2}(x_j-x_i)
$$
成立

假设原式对$n-1$阶$Vandermonde$行列式成立，即
$$
D_{n-1}=
\begin{vmatrix}
1&1&\cdots&1\\
x_2&x_3&\cdots&x_n\\
x_2^2&x_3^2&\cdots&x_n^2\\
\vdots&\vdots&&\vdots\\
x_2^{n-2}&x_3^{n-2}&\cdots&x_n^{n-2}
\end{vmatrix}=
\prod_{2\le i\lt j\le n}(x_j-x_i)
$$
只需证原式对$n$阶$Vandermonde$行列式成立即可

为此，将$D_n$的第$n-1$行的$-x_1$倍加到第$n$行，第$n-2$行$-x_1$倍加到第$n-1$行，如此作下去，直到第$1$行$-x_1$倍加到第$2$行，得
$$
\begin{align}
D_n&=
\begin{vmatrix}
1&1&1&\cdots&1\\
0&x_2-x_1&x_3-x_1&\cdots&x_n-x_1\\
0&x_2(x_2-x_1)&x_3(x_3-x_1)&\cdots&x_n(x_n-x_1)\\
\vdots&\vdots&\vdots&&\vdots\\
0&x_2^{n-2}(x_2-x_1)&x_3^{n-2}(x_3-x_1)&\cdots&x_n^{n-2}(x_n-x_1)
\end{vmatrix}\\\\
&=
1\cdot(-1)^{1+1}
\begin{vmatrix}
x_2-x_1&x_3-x_1&\cdots&x_n-x_1\\
x_2(x_2-x_1)&x_3(x_3-x_1)&\cdots&x_n(x_n-x_1)\\
\vdots&\vdots&&\vdots\\
x_2^{n-2}(x_2-x_1)&x_3^{n-2}(x_3-x_1)&\cdots&x_n^{n-2}(x_n-x_1)
\end{vmatrix}\\\\
&=(x_2-x_1)(x_3-x_1)\cdots(x_n-x_1)
\begin{vmatrix}
1&1&\cdots&1\\
x_2&x_3&\cdots&x_n\\
x_2^2&x_3^2&\cdots&x_n^2\\
\vdots&\vdots&&\vdots\\
x_2^{n-2}&x_3^{n-2}&\cdots&x_n^{n-2}
\end{vmatrix}\\\\
&=(x_2-x_1)(x_3-x_1)\cdots(x_n-x_1)\prod_{2\le i\lt j\le n}(x_j-x_i)\\
&=\prod_{1\le i\lt j\le n}(x_j-x_i)
\end{align}
$$
根据数学归纳法，证得$Vandermonde$行列式
$$
D_n=\prod_{1\le i\lt j\le n}(x_j-x_i)
$$
对任意的正整数$n(n\ge 2)$都成立