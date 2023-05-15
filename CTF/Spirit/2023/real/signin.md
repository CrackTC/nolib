#Web #HTTP #状态码 #Header

```
/
```

![[Pasted image 20230512150426.png|400]]

点击之后好像没什么反应（？

---

```html
<a href="/flag">Get Your Flag</a>
```

这个按钮是个指向`/flag`的链接

![[Pasted image 20230512151916.png|800]]

301重定向了，但是可以看到有个`Flag`头藏着，解码就好了～

![[Pasted image 20230512152025.png|800]]

---

一键获得flag

```shell
#!/bin/bash

url='http://<host>:<port>'
curl --silent --head "$url/flag" | grep Flag | sed 's/.*Flag: \(.*\)\r/\1/' | base64 -d
```