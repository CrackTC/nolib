参见 [Minyami: AbemaTV 与 Fresh Live 下载](https://www.bilibili.com/read/cv937578)

有一个 chrome 扩展 Minyami 用于生成下载命令

实测 GreenCloud 的 JP 服务器可以下载

`ssh`登录 vps

```shell
apt install npm
npm install -g minyami
```

之后下载的话在 vps 里`cd`到 smb 目录复制粘贴命令就可以愉快地屯下了

#fish #abema #minyami