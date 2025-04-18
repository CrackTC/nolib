# [HarekazeCTF2019]Avatar Uploader 1
![](<./img/Pasted image 20230216190734.png>)

```
/
```

![](<./img/Pasted image 20230216200123.png>)

啊，系二次元

![](<./img/Pasted image 20230216200251.png>)

```
upload.php
```

```php
<?php
error_reporting(0);

require_once('config.php');
require_once('lib/util.php');
require_once('lib/session.php');

$session = new SecureClientSession(CLIENT_SESSION_ID, SECRET_KEY);

// check whether file is uploaded
if (!file_exists($_FILES['file']['tmp_name']) || !is_uploaded_file($_FILES['file']['tmp_name'])) {
  error('No file was uploaded.');
}

// check file size
if ($_FILES['file']['size'] > 256000) {
  error('Uploaded file is too large.');
}

// check file type
$finfo = finfo_open(FILEINFO_MIME_TYPE);
$type = finfo_file($finfo, $_FILES['file']['tmp_name']);
finfo_close($finfo);
if (!in_array($type, ['image/png'])) {
  error('Uploaded file is not PNG format.');
}

// check file width/height
$size = getimagesize($_FILES['file']['tmp_name']);
if ($size[0] > 256 || $size[1] > 256) {
  error('Uploaded image is too large.');
}
if ($size[2] !== IMAGETYPE_PNG) {
  // I hope this never happens...
  error('What happened...? OK, the flag for part 1 is: <code>' . getenv('FLAG1') . '</code>');
}

// ok
$filename = bin2hex(random_bytes(4)) . '.png';
move_uploaded_file($_FILES['file']['tmp_name'], UPLOAD_DIR . '/' . $filename);

$session->set('avatar', $filename);
flash('info', 'Your avatar has been successfully updated!');
redirect('/');
```

需要满足一个矛盾，既要是`png`又不能是`png`

两次判断文件类型使用的方法不同，第一次使用`finfo`，而第二次使用`getimagesize`

如果上传的`png`图片不包含尺寸信息，也就是将`IHDR`之后留空便能绕过，同时也能被识别为`png`文件

![](<./img/Pasted image 20230216204726.png>)

![](<./img/Pasted image 20230216204755.png>)
