# docker 文件映射踩坑

在写一个Clash的配置转换服务的时候添加了一个`FileWatch`的功能，实时检测配置文件的更改并重新加载配置文件

在本机运行的时候好好的，封装成镜像之后在vps上通过`Docker Compose`挂载`config.yml`到容器内

```yml
version: '3'
services:
  subparser:
    image: cracktc/subparser
    ports:
      - 4447:4447
    volumes:
      - type: bind
        source: ./config.yml
        target: /app/config.yml
```

然后发现在宿主机上对`config.yml`的修改并没有生效

一开始没多想，以为是代码有问题，看到stackoverflow上有人遇到了相似的问题，以为是`dotnet`的`FileSystemWatcher`的实现问题，遂转到了`PhysicalFileProvider`并使用轮询方式检测文件更改，中间写回调可谓是一波三折

心想这应该能解决了，一放vps上又瞎了qaq

然后忽然有了一个神奇的想法，直接exec进容器，一看配置文件压根没改，就很emmm

解决方法是挂载目录而不是单个文件，这样更改就能对容器可见啦～

```yml
# config.yml在config目录下
version: '3'
services:
  subparser:
    image: cracktc/subparser
    ports:
      - 4447:4447
    volumes:
      - type: bind
        source: ./config
        target: /app/config
```

![](<./img/Pasted image 20230516160705.png>)

~~或许原来的`FileSystemWatcher`是能用的qaq~~
