对于行满秩矩阵$A_{m\times n}$，必有列满秩矩阵$B_{n\times m}$，使$AB=E$

# 证明
当$m=n$时显然成立，所以只需考虑$m<n$的情况

由$R(A)=m$，知$A$中存在$m$个列，由它们构成的$m$阶子式$|A_1|\ne0$，$A$经过适当的列的换法变换可使$A_1$位于$A$的前$m$列，即有$n$阶可逆矩阵$P$，使
$$
AP=(A_1,A_2)
$$
其中$A_1$为$m$阶可逆矩阵，令
$$
B=P
\begin{bmatrix}
A_1^{-1}\\
O
\end{bmatrix}
$$
则$R(B)=R(A_1^{-1})=m$，于是$B$为$n\times m$列满秩矩阵，且有
$$
AB=(A_1, A_2)
\begin{bmatrix}
A_1^{-1}\\
O
\end{bmatrix}
=
E
$$