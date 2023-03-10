[[square_mat|方阵]]$A$[[invertible_matrix|可逆]]的充分必要条件是存在方阵$B$，使$AB=E$（或$BA=E$），并且当$A$可逆时，有
$$B=A^{-1}$$
# 证明
## 必要性
若$A$可逆，则取$B=A^{-1}$，即有
$$
AB=E
$$
## 充分性
若存在$B$使得$AB=E$，对等式两端取[[det|行列式]]，即有
$$
|A||B|=1
$$
于是有
$$
|A|\ne0
$$
根据[[non-zero_det|这个充分必要条件]]可知，$A$可逆，于是在等式$AB=E$两边同时左乘$A^{-1}$，便得到
$$
B=A^{-1}
$$
# 推论
对于方阵$A,B$，只要有$AB=E$，则$A,B$都可逆且互为逆