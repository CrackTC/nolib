紧跟时事.jpg

![[Pasted image 20230113201736.png|500]]

这架势不会真的要复盘罢qaq

[抄作业](https://youtu.be/yfneS2R-Pn8)

具体原理就是新版本重写了鉴权，管理员的`api`鉴权代码只判断了用户提交的`token`是否存在在服务器的缓存中，然后就没有然后了，直接通过

先注册一个普通账号，进行登录

进入管理面板

![[Pasted image 20230113202029.png|1000]]

![[Pasted image 20230113202207.png|1000]]

```
authorization:"MTIzQHFxLmNvbTokMnkkMTAkSjcucmpWNXNHYkFFejlYeEFha3FYdUlBNWQxbFRiRDhReHNPbFN4eXdxNWM1bnJMbmF3aEc="
```

添加请求头

![[Pasted image 20230113202506.png]]

进入后台

```
/admin
```

![[Pasted image 20230113202604.png|1000]]

直接将`url`中的`login`改为`user`

![[Pasted image 20230113202706.png|1000]]

#Web #Vulnerabilities #v2board #token 