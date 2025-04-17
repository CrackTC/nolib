![](<./img/Pasted image 20221228105111.png>)

---
首页长这样

![](<./img/Pasted image 20221228105131.png>)

```js
$(document).ready(function(){
    $("#div1").load("check.php #p1");

        $(".close").click(function(){
        		$("#myAlert").hide();
    });	     

    $("#button1").click(function(){
    	$("#myAlert").hide();
    	guess=$("input").val();
		$.ajax({
	   type: "POST",
	   url: "check.php",
	   data: "num="+guess,
		   success: function(msg){
		     $("#div2").append(msg);
		     alertmsg = $("#flag").text(); 
		     if(alertmsg=="没抽中哦，再试试吧"){
		      $("#myAlert").attr("class","alert alert-warning");
		      if($("#new").text()=="")
		     	$("#new").append(alertmsg);
		     }
		     else{		     	
		     	$("#myAlert").attr("class","alert alert-success");
		     	if($("#new").text()=="")	
		     		$("#new").append(alertmsg);	
		     }

		 
		   }
		}); 
		$("#myAlert").show();
		$("#new").empty();
		 $("#div2").empty();
	});
});
```

```php
/check.php
```

```php
jnlWwdle9i
<?php
#这不是抽奖程序的源代码！不许看！
header("Content-Type: text/html;charset=utf-8");
session_start();
if(!isset($_SESSION['seed'])){
$_SESSION['seed']=rand(0,999999999);
}

mt_srand($_SESSION['seed']);
$str_long1 = "abcdefghijklmnopqrstuvwxyz0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ";
$str='';
$len1=20;
for ( $i = 0; $i < $len1; $i++ ){
    $str.=substr($str_long1, mt_rand(0, strlen($str_long1) - 1), 1);       
}
$str_show = substr($str, 0, 10);
echo "<p id='p1'>".$str_show."</p>";


if(isset($_POST['num'])){
    if($_POST['num']===$str){x
        echo "<p id=flag>抽奖，就是那么枯燥且无味，给你flag{xxxxxxxxx}</p>";
    }
    else{
        echo "<p id=flag>没抽中哦，再试试吧</p>";
    }
}
show_source("check.php"); 
```

使用`mt_rand`来生成字符串，存在伪随机数的安全性问题

使用`php_mt_seed`进行种子爆破

~~好的手残清了cookie~~，现在变成了`WrOeQwr5bL`

```python
str_long1 = 'abcdefghijklmnopqrstuvwxyz0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ'
s = 'WrOeQwr5bL'
for i in range(len(s)):
    ind = str_long1.find(s[i])
    print(ind, ind, 0, 61, end=' ')
```

```
58 58 0 61 17 17 0 61 50 50 0 61 4 4 0 61 52 52 0 61 22 22 0 61 17 17 0 61 31 31 0 61 1 1 0 61 47 47 0 61 
```

![](<./img/Pasted image 20221228131240.png>)

```php
<?php
mt_srand(15816320);
$str_long1 = "abcdefghijklmnopqrstuvwxyz0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ";
$str = '';
$len1 = 20;
for ($i = 0; $i < $len1; $i++) {
    $str .= substr($str_long1, mt_rand(0, strlen($str_long1) - 1), 1);
}
echo "<p id='p1'>".$str."</p>";
```

```
WrOeQwr5bLvyHUVHCqet
```

![](<./img/Pasted image 20221228131410.png>)

#Web #PHP #伪随机数 #Vulnerabilities #mt_rand