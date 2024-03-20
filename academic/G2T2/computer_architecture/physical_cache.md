**物理 Cache**：通过物理地址访问 Cache。

当 CPU 访问内存时，先通过 MMU 获得物理地址，再拿物理地址去访问 Cache。由于地址转换和访问 Cache 是串行的，访问速度会受到影响。

![[physical_cache_01.png]]
