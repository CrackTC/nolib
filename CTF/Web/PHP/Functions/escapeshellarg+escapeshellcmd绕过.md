参考 https://paper.seebug.org/164/

`escapeshellarg`会将所有`'`替换成`'\''`，最后在整个字符串两边再套上单引号，以保证该字符串作为`shell`命令的一个而非多个参数被`shell`解析

`escapeshellcmd`会将反斜杠转义，同时也转义最后一个不配对的引号

连用这两个的后果便是`escaepeshellarg`生成的反斜杠被`escapeshellcmd`转义，于是多了一个作为`shell`语法结构的`'`，反转了我们`payload`的引号内外关系，使得`payload`被作为多个参数解析

#Web #PHP #函数 #绕过 #shell 