# 定理
若$n$阶[[square_mat|方阵]]$A$的第$i$行所有元素除$a_{ij}$外其余的元素都为$0$，那么这个方阵$A$的行列式$|A|$等于$a_{ij}$与它的代数余子式$A_{ij}$的乘积，即
$$
|A|=a_{ij}A_{ij}
$$
# 证明
先证明$i=1$，$j=1$的情况，此时
$$
|A|=
\begin{vmatrix}
a_{11}&0&\cdots&0\\
a_{21}&a_{22}&\cdots&a_{2n}\\
\vdots&\vdots&&\vdots\\
a_{n1}&a_{n2}&\cdots&a_{nn}
\end{vmatrix}
$$
于是
$$
|A|=a_{11}A_{11}
$$
显然成立

对于$i,j$的其他情况，根据换法变号的性质，最终变号次数为$i+j$刚好与[[academic/G1T1/linear_algebra/1-det/2-calc/0-det_expansion/0-pre/0-def/remain_and_alremain|代数余子式]]的$(-1)^{i+j}$相符，于是定理成立