![](<./img/Pasted image 20230122094444.png>)

![](<./img/Pasted image 20230122094604.png>)

```
/robots.txt
```

![](<./img/Pasted image 20230122094713.png>)

# 相关漏洞

[ThinkPHP 6.0.0 - 6.0.1 Arbitrary File Write Vulnerability](https://community.f5.com/t5/technical-articles/thinkphp-6-0-0-6-0-1-arbitrary-file-write-vulnerability/ta-p/281591)

# 利用

![](<./img/Pasted image 20230122105834.png>)

扫目录发现`www.zip`

![](<./img/Pasted image 20230122111136.png>)

发现`/web/app/home/controller/Member.php`中存在使用`POST`方法传递的数据进行写入

```php
public function search()
{
	if (Request::isPost()){
		if (!session('?UID'))
		{
			return redirect('/home/member/login');            
		}
		$data = input("post.");
		$record = session("Record");
		if (!session("Record"))
		{
			session("Record",$data["key"]);
		}
		else
		{
			$recordArr = explode(",",$record);
			$recordLen = sizeof($recordArr);
			if ($recordLen >= 3){
				array_shift($recordArr);
				session("Record",implode(",",$recordArr) . "," . $data["key"]);
				return View::fetch("result",["res" => "There's nothing here"]);
			}

		}
		session("Record",$record . "," . $data["key"]);
		return View::fetch("result",["res" => "There's nothing here"]);
	}else{
		return View("search");
	}
}
```

对应的是这个路径

```
/home/member/search
```

![](<./img/Pasted image 20230122112023.png>)

![](<./img/Pasted image 20230122112115.png>)

修改`PHPSESSID`，最终效果是`key`的值被写入`/runtime/session/sess_<PHPSESSID>`中

![](<./img/Pasted image 20230122112342.png>)

```
/runtime/session/sess_xxx.php?cmd=phpinfo();
```

![](<./img/Pasted image 20230122113606.png>)

`antsword`连接之后使用插件绕过`disable_functions`

![](<./img/Pasted image 20230122114126.png>)

#Web #Vulnerabilities #PHP #thinkphp #disable_functions #antsword 