![[Pasted image 20221111120846.png|800]]

```
/
```

![[Pasted image 20221111120901.png|800]]

```
/Download?filename=help.docx
```

![[Pasted image 20221111120942.png|500]]

```
/Login
```

![[Pasted image 20221111121009.png|500]]

`java`实现的`web`服务真没接触过，这题算是学习了
查看大佬的wp发现这个`filename`参数实际上是要通过`POST`方法传递的（这也太离谱了）
```php
filename=help.docx
```

![[Pasted image 20221111202541.png|500]]

```php
/abc
```

![[Pasted image 20221111204042.png|800]]

服务器是`tomcat`

这里有一篇介绍tomcat目录结构的文章

https://www.jianshu.com/p/81ec9c51435e

访问`WEB-INF/web.xml`
```php
filename=WEB-INF/web.xml
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <welcome-file-list>
        <welcome-file>Index</welcome-file>
    </welcome-file-list>

    <servlet>
        <servlet-name>IndexController</servlet-name>
        <servlet-class>com.wm.ctf.IndexController</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>IndexController</servlet-name>
        <url-pattern>/Index</url-pattern>
    </servlet-mapping>

    <servlet>
        <servlet-name>LoginController</servlet-name>
        <servlet-class>com.wm.ctf.LoginController</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>LoginController</servlet-name>
        <url-pattern>/Login</url-pattern>
    </servlet-mapping>

    <servlet>
        <servlet-name>DownloadController</servlet-name>
        <servlet-class>com.wm.ctf.DownloadController</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>DownloadController</servlet-name>
        <url-pattern>/Download</url-pattern>
    </servlet-mapping>

    <servlet>
        <servlet-name>FlagController</servlet-name>
        <servlet-class>com.wm.ctf.FlagController</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>FlagController</servlet-name>
        <url-pattern>/Flag</url-pattern>
    </servlet-mapping>

</web-app>
```
`<servlet>`有两个子元素：
- `<servlet-name>`应该是用于在`web.xml`里面标识`servlet`
- `<servlet-class>`应该是用于描述`servlet`对应的控制器类

`<servlet-mapping>`有两个元素`servlet-name`和`url-pattern`，用于将特定的url路径绑定到`servlet-name`所指示的`servlet`的控制器

这道题有四个`servlet`，分别绑定到
- Index
- Login
- Download
- Flag

于是目的变为获得`com.wm.ctf.FlagController`类的内容

按照java的文件命名规则，文件名应该是`com/wm/ctf/FlagController.class`，而`com`文件夹按照tomcat的目录结构应该在`WEB-INF/classes`里
```php
filename=WEB-INF/classes/com/wm/ctf/FlagController.class
```
![[Pasted image 20221113150438.png]]
```php
filename=WEB-INF/classes/com/wm/ctf/FlagController.java
```

发现并不能下载.java后缀的文件，可能是想php那样直接被服务器执行了

回过头来看`FlagController.class`

![[Pasted image 20221113151043.png]]

这里有串base64编码的东西，猜测就是`flag`

![[Pasted image 20221113151157.png]]

#Web #tomcat #Java