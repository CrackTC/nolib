参见[[[CISCN2019 华北赛区 Day1 Web1]Dropbox]]

利用`phar`协议能够触发反序列化

`phar`的生成方法如下
```php
<?php
class Test {
	public $name = '123';
}

@unlink('phar.phar');
$phar = new Phar("phar.phar");
$phar->startBuffering();
$phar->setStub("<?php __HALT_COMPILER();?>");
$test = new Test();
$phar->setMetadata($test);
$phar->addFromString("abc.txt", "qaq");
$phar->stopBuffering();
```

#Web #PHP #phar #反序列化