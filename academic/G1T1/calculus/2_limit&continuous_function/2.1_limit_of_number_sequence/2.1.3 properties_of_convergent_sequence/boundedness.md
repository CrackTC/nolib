[[convergent|收敛]][[number_sequence|数列]]必[[bounded&unbounded|有界]]
# 证明
设数列$\{x_n\}$收敛于$a$，即$\lim_{n\to\infty}x_n=a$，由[[precise_definition_of_sequence_limit|数列极限的精确定义]]可知，对于任意给定的正数$\epsilon_0$（这里不妨取$\epsilon_0=1$，存在正整数$N$，当$n>N$时，有
$$|x_n-a|<\epsilon_0=1$$
即当$n>N$时，有
$$|x_{n}|<|a|+1$$
取$M=max\{|x_{1}|,|x_{2}|,\cdots,|x_{N}|,|a|+1\}$，则对一切正整数$n$，都有
$$|x_{n}|\le M$$
故数列$\{x_{n}\}$是有界的

# 推论
若数列$\{x_{n}\}$无界，则数列$\{x_{n}\}$必[[divergent|发散]]
## 证明
此为有界性命题的逆否命题