设$A$为$m\times n$[[matrix|矩阵]]，$B$为$n\times p$矩阵，[[partitioned_matrix|分块]]成
$$
A=
\left(
\begin{matrix}
A_{11}&\cdots&A_{1t}\\
\vdots&&\vdots\\
A_{s1}&\cdots&A_{st}
\end{matrix}
\right),\quad
B=
\left(
\begin{matrix}
B_{11}&\cdots&B_{1r}\\
\vdots&&\vdots\\
B_{t1}&\cdots&B_{tr}
\end{matrix}
\right)
$$

$$
AB=
\left(
\begin{matrix}
C_{11}&\cdots&C_{1r}\\
\vdots&&\vdots\\
C_{s1}&\cdots&C_{sr}
\end{matrix}
\right)
$$
其中$$C_{ij}=\sum^t_{k=1}{A_{ik}B_{kj}}\quad (i=1,\cdots,s;\quad j=1,\cdots,r)$$
# 条件
根据[[matrix_multiplication|矩阵乘法]]的条件，$A_{i1},A_{i2}, \cdots, A_{it}$的列数分别等于$B_{1j},B_{2j},\cdots,B_{tj}$的行数

或者更形象的说，对$A$的列的分法要与对$B$的行的分法一致