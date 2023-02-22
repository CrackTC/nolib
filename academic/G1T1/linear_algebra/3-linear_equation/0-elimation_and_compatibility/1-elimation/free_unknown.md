设非齐次线性方程组$Ax=b$，其中$A=(a_{ij})_{m\times n}$，且$R(A)=r$

不妨设$A$的前$r$列中有$r$阶非零子式，对增广矩阵$B=(A,b)$进行行的换法变换，将非零子式所在的行移动到前$r$行，再经过若干次初等行变换，将$B$化为行最简形矩阵
$$
(C,d)=
\begin{bmatrix}
1&0&\cdots&0&c_{1,r+1}&\cdots&c_{1n}&d_1\\
&1&\ddots&\vdots&\vdots&&\vdots&\vdots\\
&&\ddots&0&c_{r-1,r+1}&\cdots&c_{r-1,n}&d_{r-1}\\
&&&1&c_{r,r+1}&\cdots&c_{rn}&d_r\\
&&&&0&\cdots&0&d_{r+1}\\
&&&&0&\cdots&0&0\\
&&&&\vdots&&\vdots&\vdots\\
&&&&0&\cdots&0&0
\end{bmatrix}
$$
由于初等变换不改变矩阵的秩，所以
$$
R(A)=R(C)=r
$$
从而
$$
R(A,b)=R(C,d)=
\begin{cases}
r,&当d_{r+1}=0时,\\
r+1,&当d_{r+1}\ne0时
\end{cases}
$$
显然：

* 当$d_{r+1}\ne0$时，$R(A,b)>R(A)$，于是显然方程组无解

* 当$d_{r+1}=0$时，$R(A,b)=R(A)=r$
	* 若$r=n$，方程组有唯一解
	* 若$r<n$，方程组有无穷多个解，此时称$x_{r+1},x_{r+2},\cdots,x_{n}$为一组自由未知量