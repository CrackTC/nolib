Hint
---

[Fixed-point combinator](https://en.wikipedia.org/wiki/Fixed-point_combinator)

Solution
---

```php
<?php

highlight_file(__FILE__);

if (isset($_GET['run_around'])) {
    $run_around = $_GET['run_around'];
} else {
    die("まだまだだね～");
}

if (isset($_GET['desert'])) {
    $desert = $_GET['desert'];
} else {
    die("nonono~");
}

if (isset($_GET['hurt'])) {
    $hurt = $_GET['hurt'];
} else {
    die("almost here~");
}

$up = array(2, 5, 17, 23, 491);
$down = array(2, 31, 1847);

if ((fn ($gonna) => (fn ($give) => $give($give))(fn ($give) => $gonna(fn ($you) => $give($give)($you))))(fn ($give) => fn ($you) => !$$run_around ? 1 : $desert($you) * $give($hurt($you, 0, -1)))($up) === 1919810 &&
    (fn ($gonna) => (fn ($let) => $let($let))(fn ($let) => $gonna(fn ($you) => $let($let)($you))))(fn ($give) => fn ($you) => !$$run_around ? 1 : $desert($you) * $give($hurt($you, 0, -1)))($down) === 114514
) {
    echo $_ENV['FLAG'] ?? "Spirit{fake-flag-qwq}";
} else {
    echo "nope😡";
}
```

前面部分是个Y组合子

```php
<?php
$Y = (fn ($gonna) => (fn ($give) => $give($give))(fn ($give) => $gonna(fn ($you) => $give($give)($you))));
```

具体原理不用太在意，只要知道这个函数应用给另一个函数可以生成后一个函数的递归就行了

```php
<?php
(fn ($give) => fn ($you) => !$$run_around ? 1 : $desert($you) * $give($hurt($you, 0, -1)))
```

`$give`可以理解为后一个函数本身，`$you`是`$up`或者`$down`数组，要做的是实现乘法的递归，边界条件是`!$$run_around`，也就是`$$run_around`为空，于是推测出`$run_around`是`"you"`，`$desert`用于获取当前值，这里使用`end`函数获取数组最后一个元素，`$hurt`用于获取规模缩小后的输入，这里0和-1俩参数可以推断出是`array_slice`获取除最后一个元素外的元素数组。

```
http://127.0.0.1:8080/?run_around=you&desert=end&hurt=array_slice
```

(出题出魔怔了，逃
