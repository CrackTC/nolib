# [BJDCTF2020]Mark loves cat
![](<./img/Pasted image 20221122163014.png>)

---
首页长这样

![](<./img/Pasted image 20221122163031.png>)

炫酷.jpg

既然想不到啥方法，那就扫目录看看八

![](<./img/Pasted image 20221122163231.png>)

有`git`目录，有一个`flag.php`

康康`git`泄漏

---
`flag.php`（`lolcat`真好玩QwQ）

![](<./img/Pasted image 20221122164101.png>)

---
`index.php`
```php
<?php
include 'flag.php';

$yds = "dog";
$is = "cat";
$handsome = 'yds';

foreach($_POST as $x => $y){
    $$x = $y;
}

foreach($_GET as $x => $y){
    $$x = $$y;
}

foreach($_GET as $x => $y){
    if($_GET['flag'] === $x && $x !== 'flag'){
        exit($handsome);
    }
}

if(!isset($_GET['flag']) && !isset($_POST['flag'])){
    exit($yds);
}

if($_POST['flag'] === 'flag'  || $_GET['flag'] === 'flag'){
    exit($is);
}

echo "the flag is: ".$flag;
```

emmm，虽然但是，这个不是让`$_GET['yds']='flag'`就绕过了嘛，感觉有点像脑筋急转弯.jpg
```php
?yds=flag
```
![](<./img/Pasted image 20221123004833.png>)

#Web #PHP #代码审计 #可变变量 #目录扫描 #git泄露 