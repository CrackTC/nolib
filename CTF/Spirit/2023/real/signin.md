# Spirit CTF 2023 题解 - signin

```
/
```

![](<./img/Pasted image 20230512150426.png>)

点击之后好像没什么反应（？

---

```html
<a href="/flag">Get Your Flag</a>
```

这个按钮是个指向`/flag`的链接

![](<./img/Pasted image 20230512151916.png>)

301重定向了，但是可以看到有个`Flag`头藏着，解码就好了～

![](<./img/Pasted image 20230512152025.png>)

---

一键获得flag

```shell
#!/bin/bash

url='http://<host>:<port>'
curl --silent --head "$url/flag" | grep Flag | sed 's/.*Flag: \(.*\)\r/\1/' | base64 -d
```
