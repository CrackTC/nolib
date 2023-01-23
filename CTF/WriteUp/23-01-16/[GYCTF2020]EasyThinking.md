![[Pasted image 20230122094444.png|500]]

![[Pasted image 20230122094604.png|1000]]

```
/robots.txt
```

![[Pasted image 20230122094713.png|700]]

# 相关漏洞

[ThinkPHP 6.0.0 - 6.0.1 Arbitrary File Write Vulnerability](https://community.f5.com/t5/technical-articles/thinkphp-6-0-0-6-0-1-arbitrary-file-write-vulnerability/ta-p/281591)

# 利用

![[Pasted image 20230122105834.png|500]]

扫目录发现`www.zip`

![[Pasted image 20230122111136.png|1000]]

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

![[Pasted image 20230122112023.png|500]]

![[Pasted image 20230122112115.png|1000]]

修改`PHPSESSID`，最终效果是`key`的值被写入`/runtime/session/sess_<PHPSESSID>`中

![[Pasted image 20230122112342.png|700]]

```
/runtime/session/sess_xxx.php?cmd=phpinfo();
```

![[Pasted image 20230122113606.png|1000]]

`antsword`连接之后使用插件绕过`disable_functions`

![[Pasted image 20230122114126.png|500]]

#Web #Vulnerabilities #PHP #thinkphp #disable_functions #antsword 