形如
$$
G=
\begin{bmatrix}
E_r & O \\ 
O & O
\end{bmatrix}
=
\begin{bmatrix}
1&&&&&\\
&\ddots&&&&\\
&&1&&&\\
&&&0&\cdots&0\\
&&&\vdots&&\vdots\\
&&&0&\cdots&0
\end{bmatrix}
$$
的矩阵称为**标准形矩阵**

# 定理
设$A$为$m\times n$矩阵，则$A$必可通过有限次初等变换化为标准型矩阵

## 证明
若$A$为[[zero_mat|零矩阵]]，则定理显然成立，此时$r=0$，否则，必可通过行列的换法变换使得第一行、第一列元素$d$不为$0$，以$\frac{1}{d}$乘第一行，化$(1,1)$元为$1$，再经过适当的行、列消法变换，将矩阵化为如下形式：
$$
B=\begin{bmatrix}1 & 0 & \cdots & 0 \\ 0 & b_{22} & \cdots & b_{2n} \\ \vdots & \vdots & & \vdots\\ 0 & b_{m2} & \cdots & b_{mn}\end{bmatrix}
$$
如果$b_{ij}(i=2,3,\cdots,m;j=2,3,\cdots,n)$全为$0$，则$B$便是标准形矩阵。如若不然，则问题简化为对$B$的子矩阵进行相同的处理，最终必能得到一个标准形矩阵。