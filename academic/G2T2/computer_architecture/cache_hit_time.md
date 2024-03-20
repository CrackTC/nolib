# 容量小、结构简单的 Cache
显然，Cache容量越小，结构越简单，命中时间就越短

# 虚拟 Cache
- [[physical_cache|物理 Cache]]
- [[virtual_cache|虚拟 Cache]]
- [[vindex_ptag|虚拟索引-物理标识]]

# Cache 访问流水化

实际上是提高访问 Cache 的带宽

# 踪迹 Cache
普通的指令 Cache 存放的是静态的指令序列，而踪迹 Cache 存放的是 CPU 执行过的动态指令序列，其中包含了由分支预测展开过的指令。

缺陷是相同的指令序列可能被当作条件分支的不同选择被存放多次。
