#Web #HTTP #Header 

```
/
```

![[Pasted image 20230507233337.png|800]]

---

Part #1
===
![[Pasted image 20230507233553.png|500]]

这里是对`User-Agent`请求头的修改，一般各大搜索引擎的蜘蛛都会带有特定的UA信息

[关于UA](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent)

[百度的说法](https://help.baidu.com/question?prod_id=99&class=0&id=3001)

![[Pasted image 20230507234519.png|800]]

```shell
curl --user-agent Baiduspider http://<host>:<port>/verify
```

Part #2
===

![[Pasted image 20230507235025.png|800]]

为了声明自己从哪个网址到这里来，浏览器发送`Referer`头

`Referer`的一个常见的作用是防盗链。比如一些图片网站会验证`Referer`头，如果不是自己的网站，就直接403
~~（一种可行的反制措施是进行图片反代，推销下最近写的[反代服务器（就当无事发生）](javascript:void(0);)）~~

```shell
curl --user-agent Baiduspider --referer https://www.baidu.com http://<host>:<port>/verify
```

Part #3
===

![[Pasted image 20230508003418.png|800]]

这里是改`Cookie`，参考Guidebook

要求是存在名为`price`的`Cookie`（hint），脑洞属实开大了>\_<

```shell
curl --user-agent Baiduspider --referer https://www.baidu.com --cookie price= http://<host>:<port>/verify
```

![[Pasted image 20230508004015.png|800]]
