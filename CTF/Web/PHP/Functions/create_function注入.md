参见 [[[BJDCTF2020]EzPHP（学习）]]

由于`php`的`create_function`通过字符串拼接后调用`eval`实现函数的创建，于是可以通过`}`来使后面的代码变成顶级语句让`eval`执行

#Web #PHP #函数 #create_function注入