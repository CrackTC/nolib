由[[ele_row_trans_composited|这个]]可以得到另一种求[[invertible_matrix|逆矩阵]]的方法：对于可逆矩阵$A$，设有[[ele_mat|初等矩阵]]$P_1,P_2,\cdots,P_t$，使
$$
P_t\cdots P_2P_1A=E
$$
则
$$
P_t\cdots P_2P_1=A^{-1}
$$
利用[[part_mat|分块矩阵]]的形式，可以将两式合并为
$$
P_t\cdots P_2P_1\begin{pmatrix}A&E\end{pmatrix}=\begin{pmatrix}E&A^{-1}\end{pmatrix}
$$
当然，采取初等列变换的形式也是可以滴

貌似会成为非常有用的东西呢www