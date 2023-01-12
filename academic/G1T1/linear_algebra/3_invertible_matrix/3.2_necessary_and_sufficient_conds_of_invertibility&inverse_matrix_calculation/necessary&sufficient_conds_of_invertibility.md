[[square_matrix|方阵]]$A$[[invertible_matrix|可逆]]的充分必要条件是$|A|\ne0$，并且当$A$可逆时，有
$$
A^{-1}=\frac{1}{|A|}A^*
$$
# 证明
### 必要性
若$A$可逆，则存在逆矩阵$A^{-1}$，使得$AA^{-1}=E$，在等式两端取[[determinant|行列式]]，根据[[multiplication_properties|行列式的运算规律]]便有
$$
|A||A^{-1}|=1
$$
于是必有
$$
|A|\ne0
$$
### 充分性
若$|A|\ne0$，由[[properties_of_adjoint_matrix|伴随矩阵的性质]]可得
$$
A(\frac{1}{|A|}A^*)=(\frac{1}{|A|}A^*)A=E
$$
因此由[[invertible_matrix|可逆矩阵的定义]]可知，方阵$A$可逆，并且
$$
A^{-1}=\frac{1}{|A|}A^*
$$
由此，原式不但是方阵$A$可逆的充分必要条件，也给出了求$A^{-1}$的一种方法