# 定理
矩阵的秩等于行秩，也等于列秩

# 证明
感觉没啥好证.jpg，总之就是将向量组和它组成的矩阵的相关概念对应上就理解了

# 定理
设$A$，$B$均为$m\times n$矩阵，则
$$
R(A+B)\le R(A)+R(B)
$$

# 证明
显然$A+B$的列向量可以由$A$的列向量组和$B$的列向量组线性表示，设$R(A)=s,R(B)=t$，不妨设$\alpha_1,\alpha_2,\cdots,\alpha_s$是$A$的列向量组的一个极大无关组，$\beta_1,\beta_2,\cdots,\beta_t$是$B$的列向量组的一个极大无关组

由于向量组和它的极大无关组等价，由传递性知$A+B$的列向量组可由向量组$\alpha_1,\alpha_2,\cdots,\alpha_s,\beta_1,\beta_2,\cdots,\beta_t$线性表示，于是
$$
\begin{align}
R(A+B)&=(A+B)的列秩\le R(\alpha_1,\cdots,\alpha_s,\beta_1,\cdots,\beta_t)\\
&\le s+t\\
&=R(A)+R(B)
\end{align}
$$

# 定理
设$A$为$m\times n$矩阵，$B$为$n\times p$矩阵，则
$$
R(AB)\le min\{R(A),R(B)\}
$$

# 证明
不证，维都降了还想升回来？😡