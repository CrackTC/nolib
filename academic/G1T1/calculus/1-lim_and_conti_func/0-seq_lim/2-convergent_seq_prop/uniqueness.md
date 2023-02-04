若[[seq|数列]]$\{x_n\}$[[convergent|收敛]]，则其[[seq_lim_precise_def|极限]]唯一
# 证明
反证法，假设数列$\{x_n\}$收敛，但有两个不同的极限值$a$和$b$，不妨设$a>b$，取$\epsilon_0=\frac{a-b}{2}$，由$$\lim_{n\to\infty}x_n=a$$
可知，对于上述$\epsilon_0$，$\exists N_1$，当$n>N_1$时，有
$$|x_n-a|<\epsilon_0=\frac{a-b}{2}$$
即
$$x_n>\frac{a+b}{2}\tag{1}$$
再由
$$\lim_{n\to\infty}x_n=b$$
可知，对于上述$\epsilon_0>0$，$\exists N_2$，当$n>N_2$时，有
$$|x_n-b|<\epsilon_0=\frac{a-b}{2}$$
即
$$x_n<\frac{a+b}{2}\tag{2}$$
取$N=max \{N_1,N_2\}$则当$n>N$时，$(1)$，$(2)$两式同时成立，这显然是矛盾的，因此，收敛数列的极限时唯一的