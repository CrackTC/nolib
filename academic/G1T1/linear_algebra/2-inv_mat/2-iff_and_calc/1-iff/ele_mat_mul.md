[[square_mat|方阵]]$A$[[invertible_matrix|可逆]]的充分必要条件是$A$可以表示为有限个初等矩阵的乘积

# 证明
## 必要性
若$A$可逆，在[[ele_trans_composited|这个东东]]的基础上，由[[invertible_matrix|逆矩阵定义]]中所述，[[ele_mat|初等矩阵]]可逆，且其逆矩阵仍为初等矩阵可得下面这个形式
$$
A=P_1^{-1}P_2^{-1}\cdots P_s^{-1}Q_t^{-1}Q_{t-1}^{-1}\cdots Q_1^{-1}
$$
即为所证

## 充分性
设有初等矩阵$P_1,P_2,\cdots,P_t$，使得
$$
A=P_1P_2\cdots P_t
$$
因为初等矩阵可逆，所以他们的乘积也可逆，所以$A$可逆