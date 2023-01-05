![[Pasted image 20221117144136.png|500]]

---
首页长这样
![[Pasted image 20221117144322.png]]

`nginx`配置文件
```nginx
server {
  listen 8080 default_server;
  server_name nginx;

  location / {
    set $args "${args}&proxy=nginx";
    proxy_pass http://web:3000;
  }
}
```

```js
const app = require("express")();

const FLAG = process.env.FLAG ?? "SECCON{dummy}";
const PORT = 3000;

app.get("/", (req, res) => {
  req.query.proxy.includes("nginx")
    ? res.status(400).send("Access here directly, not via nginx :(")
    : res.send(`Congratz! You got a flag: ${FLAG}`);
});

app.listen({ port: PORT, host: "0.0.0.0" }, () => {
  console.log(`Server listening at ${PORT}`);
});

```
尝试过构造参数数组，甚至查过`express`和`qs`的文档，明明是签到题却没有做出来，属实心态崩了呜呜呜

---
今天看到油管上有佬放出了wp，发现属实是自己想复杂了
可能是`nginx`对参数长度的限制，只要我们构造的`url`长度足够长，又不至于引发`414`错误，就能把`nginx`反代中附加的`proxy`参数顶掉
所以`payload`为
```php
/?proxy=a&proxy=a&proxy=a...&proxy=a
```
![[Pasted image 20221117152054.png]]

---
**Edited** 受到了大佬的指点，这题当时其实没走偏，具体原理是`express`调用的`qs`处理参数个数上限为1000个，所以就被顶掉了QwQ，看来要素察觉能力有待加强
![[Pasted image 20221118091053.png]]
#Web #nodejs #express #qs #URL #URI #HTTP #nginx 