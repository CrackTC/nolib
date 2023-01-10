设$\lim_{n\to\infty}x_{n}=a$，$\lim_{n\to\infty}y_{n}=b$，则有
$$\lim_{n\to\infty}\frac{x_{n}}{y_{n}}=\frac{\lim_{n\to\infty}x_{n}}{\lim_{n\to\infty}y_{n}}=\frac{a}{b}(b\ne0)$$
# 证明
不妨设$b>0$

由于已证[[multiplication_of_sequence_limit|数列极限的乘法]]，因此问题转化为求证
$$\lim_{n\to\infty}\frac{1}{y_{n}}=\frac{1}{b}$$
由[[precise_definition_of_sequence_limit|数列极限的精确定义]]可知，需要求证对于任意$\epsilon>0$，存在正整数$N>0$，对于任意$n>N$，$|\frac{1}{y_{n}}-\frac{1}{b}|<\epsilon$

由$\lim_{n\to\infty}y_{n}=b$及[[guaranteed_number|收敛数列的保号性]]可知，存在$0<m<b$，使得存在正整数$N_{1}$，对任意$n>N_{1}$，都有
$$y_{n}>m$$
同时，对于任意$\epsilon>0$，必有$bm\epsilon>0$，于是存在正整数$N_2$，对任意$n>N_{2}$，都有
$$|y_{n}-b|<bm\epsilon$$
于是可取$N=max\{N_{1},N_{2}\}$，对于任意$n>N$，都有
$$
\begin{align*}
|\frac{1}{y_{n}}-\frac{1}{b}|&=|\frac{b-y_{n}}{y_{n}b}|\\
&< |\frac{bm\epsilon}{bm}|\\
&= \epsilon
\end{align*}
$$