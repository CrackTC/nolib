数据的两种写入策略：
- 写直达 (write-through)：同时更新cache和下一级存储器，保证数据一致性。但存在性能问题，可通过写缓冲缓解。
    - 写分配 (write-allocate)：遇到miss时，读入cache并写入。
    - 写不分配 (write-no-allocate)：遇到miss时，直接写入下一级存储器。对于大量初始化的数据，可减少cache污染。
- 写回 (write-back)：只更新cache，当写入遇到dirty块时将其写入，也可以通过写缓冲加速下一级写入
