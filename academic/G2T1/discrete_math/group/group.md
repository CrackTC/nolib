
# 定义

给定[[set|集合]]$G$，以及$G$上的一个[[binop|二元运算]]$\circ:G\times G\to G$，若它满足以下条件：

1. 结合律：$\forall a,b,c\in G, (a\circ b)\circ c=a\circ(b\circ c)$
2. 单位元：$\exists e\in G, \forall a\in G, a\circ e=e\circ a=a$
3. 逆元：$\forall a\in G, \exists a^{-1}\in G, a\circ a^{-1}=a^{-1}\circ a=e$

则称$(G,\circ)$为一个**群**。

# 等价定义

由于群的定义中包含了结合律，关于逆元和单位元的条件可以被简化为单侧条件：

- 左单位元：$\forall a\in G, e\circ a=a$\
  左逆元：$\forall a\in G, \exists a^{-1}\in G, a^{-1}\circ a=e$

- 右单位元：$\forall a\in G, a\circ e=a$\
  右逆元：$\forall a\in G, \exists a^{-1}\in G, a\circ a^{-1}=e$
