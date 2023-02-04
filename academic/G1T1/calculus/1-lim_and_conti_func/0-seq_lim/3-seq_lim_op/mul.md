设$\lim_{n\to\infty}x_{n}=a$，$\lim_{n\to\infty}y_{n}=b$，则有
$$\lim_{n\to\infty}x_{n}y_{n}=\lim_{n\to\infty}x_{n}\cdot\lim_{n\to\infty}y_{n}=ab$$
# 证明
由$\lim_{n\to\infty}x_{n}=a$，可知数列$\{x_{n}\}$[[bounded_and_unbounded|有界]]，即存在$M>0$，对一切$n$有$|x_{n}|\le M$

当$b=0$时，$\forall\epsilon>0$，由$\lim_{n\to\infty}y_{n}=0$，对于$\frac{\epsilon}{M}>0$，$\exists N$，当$n>N$时，有
$$|y_{n}|<\frac{\epsilon}{M}$$
于是，当$n\le N$时，有
$$|x_{n}y_{n}-ab|=|x_{n}||y_{n}|<M\cdot \frac{\epsilon}{M}=\epsilon$$
则有
$$\lim_{n\to\infty}x_{n}y_{n}=ab$$
当$b\ne0$时，$\forall\epsilon>0$，由$\lim_{n\to\infty}x_{n}=a$，对于$\frac{\epsilon}{2|b|}>0$，存在正整数$N_{1}$，当$n>N_{1}$时，有
$$|x_{n}-a|<\frac{\epsilon}{2|b|}$$
再由$\lim_{n\to\infty}y_{n}=b$，对于$\frac{\epsilon}{2M}>0$，存在正整数$N_{2}$，当$n>N_{2}$时，有
$$|y_{n}-b|<\frac{\epsilon}{2M}$$
取$N=max\{N_{1},N_{2}\}$，当$n>N$时，有
$$
\begin{align*}
|x_{n}y_{n}-ab|&=|x_{n}y_{n}-x_{n}b+x_{n}b-ab|\\
&\le |x_{n}||y_{n}-b|+|b||x_{n}-a|\\
&< M\cdot \frac{\epsilon}{2M}+|b|\cdot \frac{\epsilon}{2|b|}=\epsilon
\end{align*}
$$
因此有
$$\lim_{n\to\infty}x_{n}y_{n}=ab$$
