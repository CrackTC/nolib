# 例
计算[[determinant|行列式]]
$$
D=
\begin{vmatrix}
	1+x  &1    &1    &1\\
	1    &1-x  &1    &1\\
	1    &1    &1+y  &1\\
	1    &1    &1    &1-y
\end{vmatrix}
$$
# 解
当$x=0$或$y=0$时，则$D=0$，下面假设$xy\ne0$时，根据定理$3.1$，我们把$D$加上一行和一列，并保持行列式不变
$$
D=
\begin{vmatrix}
1 &1 &1 &1 &1\\
0 &1+x &1 &1 &1\\
0 &1 &1-x &1 &1\\
0 &1 &1 &1+y &1\\
0 &1 &1 &1 &1-y\\
\end{vmatrix}
\xlongequal[-r_1+r_4\atop-r_1+r_5]{-r_1+r_2\atop-r_1+r_3}
\begin{vmatrix*}[r]
1 &1 &1 &1 &1\\
-1 &x &0 &0 &0\\
-1 &0 &-x &0 &0\\
-1 &0 &0 &y &0\\
-1 &0 &0 &0 &-y
\end{vmatrix*}
\xlongequal[\frac1yc_4+c_1\atop-\frac1yc_5+c_1]{\frac1xc_2+c_1\atop-\frac1xc_3+c_1}
\begin{vmatrix*}[r]
1 &1 &1 &1 &1\\
0 &x &0 &0 &0\\
0 &0 &-x &0 &0\\
0 &0 &0 &y &0\\
0 &0 &0 &0 &-y
\end{vmatrix*}=
x^2y^2
$$
这种方法巧妙地反用[[theorem_of_determinant_expansion_by_row(column)|行列式按行（列）展开定理]]，我们形象地称这种计算行列式的方法为**加边法**

#解题技巧 #线性代数