设$\lim_{n\to\infty}x_{n}=a$，$\lim_{n\to\infty}y_{n}=b$，则有
$$\lim_{n\to\infty}(x_{n}\pm y_{n})=\lim_{n\to\infty}x_{n}\pm\lim_{n\to\infty}y_{n}=ab$$
# 证明
$\forall\epsilon>0$，由$\lim_{n\to\infty}x_{n}=a$，对于$\frac{\epsilon}{2}>0$，存在正整数$N_{1}$，当$n>N_{1}$时，有
$$|x_{n}-a|<\frac{\epsilon}{2}$$
由$\lim_{n\to\infty}y_{n}=b$，对于$\frac{\epsilon}{2}>0$，存在正整数$N_{2}$，当$n>N_{2}$时，有
$$|y_{n}-b|<\frac{\epsilon}{2}$$
取$N=max\{N_{1},N_{2}\}$，则当$n>N$时，有
$$|x_{n}+y_{n}-(a+b)|\le|x_{n}-a|+|y_{n}-b|<\frac{\epsilon}{2}+\frac{\epsilon}{2}=\epsilon$$
所以
$$\lim_{n\to\infty}(x_{n}+y_{n})=a+b$$