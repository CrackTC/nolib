# 传数组绕过
![[Pasted image 20221031211750.png]]

诸如此类判断可通过构造数组绕过
```php
/?a[]=0&b[]=1
```
原理是`md5`函数无法处理数组，所以返回的值相同

---
# 强碰撞绕过
```php
if ((string)$_POST['a'] !== (string)$_POST['b'] && md5($_POST['a']) === md5($_POST['b'])) {}
```
像这种判断，`a`和`b`被强制转换成了`string`类型，传数组的话结果都为`"Array"`，于是无法通过第一个判断，这时就需要用到`md5`强碰撞来绕过
```php
a=%4d%c9%68%ff%0e%e3%5c%20%95%72%d4%77%7b%72%15%87%d3%6f%a7%b2%1b%dc%56%b7%4a%3d%c0%78%3e%7b%95%18%af%bf%a2%00%a8%28%4b%f3%6e%8e%4b%55%b3%5f%42%75%93%d8%49%67%6d%a0%d1%55%5d%83%60%fb%5f%07%fe%a2
&b=%4d%c9%68%ff%0e%e3%5c%20%95%72%d4%77%7b%72%15%87%d3%6f%a7%b2%1b%dc%56%b7%4a%3d%c0%78%3e%7b%95%18%af%bf%a2%02%a8%28%4b%f3%6e%8e%4b%55%b3%5f%42%75%93%d8%49%67%6d%a0%d1%d5%5d%83%60%fb%5f%07%fe%a2
```
需要注意的是，`POST`时要将`Content-Type`头设为`application/x-www-form-urlencoded`，告诉服务器这段`POST`数据已经`url`编码过了

---
# `md5`与字符串本身弱比较相等
```php
$md5 = $_GET['md5'];
if ($md5 == md5($md5)) {
	echo('success');
}
die('failed');
```

由于以`0e`打头的字符串在弱类型比较时等价于`0`，所以只需暴力计算出自身`0e`打头，`md5`值也为`0e`打头的字符串即可
```php
md5=0e215962017
```

#Web #PHP #bypass #function #md5