对于$n$阶[[square_matrix|方阵]]$A$的[[determinant|行列式]]$|A|$，其任意一行（列）的所有元素与另一行（列）对应元素的[[2.3.1_minor&cofactor_of_determinant_element|代数余子式]]乘积之和等于零。即当$i\ne j$时，有
$$
\begin{align}
a_{i1}A_{j1}+a_{i2}A_{j2}+\cdots+a_{in}A_{jn}=0\\
a_{1i}A_{1j}+a_{2i}A_{2j}+\cdots+a_{ni}A_{nj}=0
\end{align}
$$
# 证明
将$|A|$按第$j$行展开，有
$$
\begin{vmatrix}
a_{11} &a_{12} &\cdots &a_{1n}\\
\vdots &\vdots &       &\vdots\\
a_{i1} &a_{i2} &\cdots &a_{in}\\
\vdots &\vdots &       &\vdots\\
a_{j1} &a_{j2} &\cdots &a_{jn}\\
\vdots &\vdots &       &\vdots\\
a_{n1} &a_{n2} &\cdots &a_{nn}
\end{vmatrix}=
a_{j1}A_{j1}+a_{j2}A_{j2}+\cdots+a_{jn}A_{jn}
$$
在上面等式的两端令$a_{jk}=a_{ik}(k=1,2,\cdots,n)$，可得
$$
\begin{vmatrix}
a_{11} &a_{12} &\cdots &a_{1n}\\
\vdots &\vdots &       &\vdots\\
a_{i1} &a_{i2} &\cdots &a_{in}\\
\vdots &\vdots &       &\vdots\\
a_{i1} &a_{i2} &\cdots &a_{in}\\
\vdots &\vdots &       &\vdots\\
a_{n1} &a_{n2} &\cdots &a_{nn}
\end{vmatrix}=
a_{i1}A_{j1}+a_{i2}A_{j2}+\cdots+a_{in}A_{jn}
$$
当$i\ne j$时，上式左端行列式的第$i$行与第$j$行完全相同，故行列式为零，即
$$
a_{i1}A_{j1}+a_{i2}A_{j2}+\cdots+a_{in}A_{jn}=0,\quad i\ne j
$$
类似可得有关列的结果
$$
a_{1i}A_{1j}+a_{2i}A_{2j}+\cdots+a_{ni}A_{nj}=0,\quad i\ne j
$$
综合这个推论和[[theorem_of_determinant_expansion_by_row(column)|行列式按行（列）展开定理]]可得：
$$
\begin{align}
a_{i1}A_{j1}+a_{i2}A_{j2}+\cdots+a_{in}A_{jn}&=
\begin{cases}
|A|&当i=j\\
0&当i\ne j
\end{cases}\\
a_{1i}A_{1j}+a_{2i}A_{2j}+\cdots+a_{ni}A_{nj}&=
\begin{cases}
|A|&当i=j\\
0&当i\ne j
\end{cases}
\end{align}
$$
或简单表示为
$$
\sum_{k=1}^{n}a_{ik}A_{jk}=\delta_{ij}|A|,\quad \sum_{k=1}^{n}a_{ki}A_{kj}=\delta_{ij}|A|
$$