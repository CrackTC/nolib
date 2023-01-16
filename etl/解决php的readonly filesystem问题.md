# 原因
`php-fpm`的`systemd`服务默认开启了`ProtectSystem=full`选项，使得网站根目录所在的`/usr`对`php-fpm`只读

# 解决方法
假设网站根目录为`/usr/share/nginx/html/`

```sh
sudo systemctl edit php-fpm.service
```

添加

```systemd
[Service]
ReadWritePaths=/usr/share/nginx/html/
```

重启服务

```sh
sudo systemctl restart php-fpm.service
```

即可

#php #workaround #systemd #linux #readonly