参见 [[[CISCN2019 华北赛区 Day1 Web2]ikun]]
可以通过为类定义`__reduce__`魔术方法来控制类的对象的序列化
```python
class payload(object):
	def __reduce__(self):
		return (<function you want to call>, (<arguments you want to pass>,))
```

如果需要进行命令执行，应该使用`commands.getoutput`，`os.system`返回值是命令执行的返回值，而不是命令的输出

#Web #python #pickle #魔术方法 #RCE #反序列化 