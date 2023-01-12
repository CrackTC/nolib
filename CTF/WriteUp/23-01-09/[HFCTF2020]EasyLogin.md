![[Pasted image 20230112122607.png|500]]

---

```
/login
```

![[Pasted image 20230112122641.png|500]]

注册一个账号，在`/home`发现一个 "GET FLAG" 按钮

![[Pasted image 20230112124628.png|500]]

按下之后提示 "permission denied"

```
/static/js/app.js
```

```js
/**
 *  或许该用 koa-static 来处理静态文件
 *  路径该怎么配置？不管了先填个根目录XD
 */

function login() {
    const username = $("#username").val();
    const password = $("#password").val();
    const token = sessionStorage.getItem("token");
    $.post("/api/login", {username, password, authorization:token})
        .done(function(data) {
            const {status} = data;
            if(status) {
                document.location = "/home";
            }
        })
        .fail(function(xhr, textStatus, errorThrown) {
            alert(xhr.responseJSON.message);
        });
}

function register() {
    const username = $("#username").val();
    const password = $("#password").val();
    $.post("/api/register", {username, password})
        .done(function(data) {
            const { token } = data;
            sessionStorage.setItem('token', token);
            document.location = "/login";
        })
        .fail(function(xhr, textStatus, errorThrown) {
            alert(xhr.responseJSON.message);
        });
}

function logout() {
    $.get('/api/logout').done(function(data) {
        const {status} = data;
        if(status) {
            document.location = '/login';
        }
    });
}

function getflag() {
    $.get('/api/flag').done(function(data) {
        const {flag} = data;
        $("#username").val(flag);
    }).fail(function(xhr, textStatus, errorThrown) {
        alert(xhr.responseJSON.message);
    });
}
```

注释提示使用了`koa-static`处理静态文件访问，并且把目录设置为web根目录

`getflag`通过`/api/flag`获取内容，对于MVC架构，业务逻辑的处理一般放在`controllers`中

因而猜测对`api`的处理由`/controllers/api.js`完成

```
/controllers/api.js
```

```js
const crypto = require('crypto');
const fs = require('fs')
const jwt = require('jsonwebtoken')

const APIError = require('../rest').APIError;

module.exports = {
    'POST /api/register': async (ctx, next) => {
        const {username, password} = ctx.request.body;
        if(!username || username === 'admin'){
            throw new APIError('register error', 'wrong username');
        }
        if(global.secrets.length > 100000) {
            global.secrets = [];
        }
        const secret = crypto.randomBytes(18).toString('hex');
        const secretid = global.secrets.length;
        global.secrets.push(secret)
        const token = jwt.sign({secretid, username, password}, secret, {algorithm: 'HS256'});
        ctx.rest({
            token: token
        });
        await next();
    },
    'POST /api/login': async (ctx, next) => {
        const {username, password} = ctx.request.body;
        if(!username || !password) {
            throw new APIError('login error', 'username or password is necessary');
        }
        const token = ctx.header.authorization || ctx.request.body.authorization || ctx.request.query.authorization;
        const sid = JSON.parse(Buffer.from(token.split('.')[1], 'base64').toString()).secretid;
        console.log(sid)
        if(sid === undefined || sid === null || !(sid < global.secrets.length && sid >= 0)) {
            throw new APIError('login error', 'no such secret id');
        }
        const secret = global.secrets[sid];
        const user = jwt.verify(token, secret, {algorithm: 'HS256'});
        const status = username === user.username && password === user.password;
        if(status) {
            ctx.session.username = username;
        }
        ctx.rest({
            status
        });
        await next();
    },
    'GET /api/flag': async (ctx, next) => {
        if(ctx.session.username !== 'admin'){
            throw new APIError('permission error', 'permission denied');
        }
        const flag = fs.readFileSync('/flag').toString();
        ctx.rest({
            flag
        });
        await next();
    },
    'GET /api/logout': async (ctx, next) => {
        ctx.session.username = null;
        ctx.rest({
            status: true
        })
        await next();
    }
};
```

`node.js`的`jwt.verify`在`secret`为空时无视第三个参数给定的`algorithm`，转而采用`none`作为签名方式

于是需要构造`sid`使得`globals.secrets[sid]`为空

```js
if(sid === undefined || sid === null || !(sid < global.secrets.length && sid >= 0)) {
    throw new APIError('login error', 'no such secret id');
}
```

利用`js`弱类型比较，`[]`在数值比较中隐式转换为`0`的特性实现绕过

![[Pasted image 20230112143336.png|1000]]

更改`alg`为`none`，`secretid`为`[]`，`username`为`admin`，最终形式如下

![[Pasted image 20230112143545.png|1000]]

![[Pasted image 20230112143624.png|500]]

成功登录

![[Pasted image 20230112143648.png|500]]

```
/api/flag
```

![[Pasted image 20230112143941.png|500]]

#Web #nodejs #koa #MVC #源码泄漏 #jwt