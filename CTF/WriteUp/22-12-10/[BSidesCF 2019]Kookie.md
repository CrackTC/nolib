# [BSidesCF 2019]Kookie
![](<./img/Pasted image 20221216185311.png>)

https://buuoj.cn/challenges#[BSidesCF%202019]Kookie

---
首页长这样

![](<./img/Pasted image 20221216185330.png>)

你所具有的特殊能力使你听到了神秘的声音，声音指引你按照提示输入用户名`cookie`和密码`monster`

![](<./img/Pasted image 20221216185613.png>)

当你在访问网站时，网站会在你的计算机上保存一些小的文本文件，这些文件就叫做cookie。网站通过这些cookie来记住你的偏好设置、登录状态、购物车内容等信息，以便你下次访问时无需重新输入或选择。

那么该如何拿到cookie呢

我们可以在浏览器窗口按下F12键召唤开发者工具

* 对于chromium世界（360安全浏览器，QQ浏览器与之相通），它躲在应用选项卡下
* 对于firefox世界，它藏在存储选项卡下

![](<./img/Pasted image 20230410214411.png>)

![](<./img/Pasted image 20230410214915.png>)

在这里，登陆后名为`username`的`cookie`被设置

![](<./img/Pasted image 20221216185811.png>)

尝试直接修改`username`为`admin`

今天的服务器酱很善良，只要我们告诉她自己是管理员，她就大方的告诉了我们flag

.![](<./img/Pasted image 20221216185850.png>)

成功拿到了flag~

#Web #HTTP #Header 