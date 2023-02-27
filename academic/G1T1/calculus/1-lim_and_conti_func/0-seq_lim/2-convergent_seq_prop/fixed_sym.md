设$\lim_{n\to\infty}x_{n}=a$，$\lim_{n\to\infty}y_{n}=b$，且$a>b$，则存在正整数$N$，当$n>N$时，有$x_{n}>y_{n}$
# 证明
对于正数$\epsilon_{0}=\frac{a-b}{2}$，由$\lim_{n\to\infty}x_{n}=a$，存在正整数$N_{1}$，当$n>N_{1}$时，有
$$|x_{n}-a|<\epsilon_{0}=\frac{a-b}{2}$$
即有
$$x_{n}>\frac{a+b}{2}$$
再由$\lim_{n\to\infty}y_{n}=b$，对于$\epsilon_{0}=\frac{a-b}{2}>0$，存在正整数$N_{2}$，当$n>N_{2}$时，有
$$|y_{n}-b|<\epsilon_{0}=\frac{a-b}{2}$$
即有
$$y_{n}<\frac{a+b}{2}$$
取$N=max\{N_{1},N_{2}\}$，则当$n>N$时，有
$$x_{n}>\frac{a+b}{2}>y_{n}$$
# 推论
## 取$y_{n}=b$得
若$lim_{n\to\infty}x_{n}=a$，且$a>b$（或$a<b$），则存在正整数$N$，当$n>N$时，有$x_{n}>b$（或$x_{n}<b$）
## 再取$b=0$得
若$\lim_{n\to\infty}x_{n}=a$，且$a>0$（或$a<0$），则存在正整数$N$，当$n>N$时，有$x_{n}>0$（或$x_{n}<0$）
## 反证法可得
设$\lim_{n\to\infty}x_{n}=a$，$\lim_{n\to\infty}y_{n}=b$，且存在正整数$N$，当$n>N$时，$x_{n}\ge y_{n}$（或$x_{n}\le y_{n}$），则有$a\ge b$（或$a\le b$）
## 以及
若$\lim_{n\to\infty}x_{n}=a$，且存在正整数$N$，当$n>N$时$x_{n}\ge 0$（或$x_{n}\le 0$），则有$a\ge 0$（或$a\le 0$）