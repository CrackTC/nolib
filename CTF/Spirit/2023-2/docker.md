
# docker安装及容器部署（以ubuntu下部署dvwa靶场为例）

## 1. 安装docker

### 可选前置任务：更换apt源

```shell
# 备份
sudo mv /etc/apt/sources.list /etc/apt/sources.list.bak
```

```shell
echo 'deb https://mirrors.jlu.edu.cn/ubuntu/ jammy main restricted
deb https://mirrors.jlu.edu.cn/ubuntu/ jammy-updates main restricted
deb https://mirrors.jlu.edu.cn/ubuntu/ jammy universe
deb https://mirrors.jlu.edu.cn/ubuntu/ jammy-updates universe
deb https://mirrors.jlu.edu.cn/ubuntu/ jammy multiverse
deb https://mirrors.jlu.edu.cn/ubuntu/ jammy-updates multiverse
deb https://mirrors.jlu.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
deb https://mirrors.jlu.edu.cn/ubuntu jammy-security main restricted
deb https://mirrors.jlu.edu.cn/ubuntu jammy-security universe
deb https://mirrors.jlu.edu.cn/ubuntu jammy-security multiverse' | sudo tee /etc/apt/sources.list
```

### 主线任务

```shell
sudo apt update
```

```shell
sudo apt install docker.io
```

## 2. dvwa部署

![[docker-1.png]]

搜索dvwa靶场可用的docker镜像

### tl;dr

readme抄作业

![[docker-2.png]]

```shell
sudo docker run --rm -it -p 80:80 vulnerables/web-dvwa
```


### 命令/选项/参数解读

- `run` 运行容器，如果本地没有镜像，会自动从docker hub下载
- `--rm` 退出时删除容器
- `-it` 交互式运行
- `-p 80:80` 将容器的80端口映射到主机的80端口，这样就能通过主机的80端口访问容器的80端口
- `vulnerables/web-dvwa:latest` 镜像名:版本号, latest即为最新版本

不严谨的解释：docker镜像像是存档点，搭建和配置环境这种累活人家在存档前帮你搞好啦，运行容器读取存档点就行，开箱即用，或者只需少量配置

### 测试

打开浏览器，访问`localhost:80`

![[image_2023-08-27-20-08-28]]
