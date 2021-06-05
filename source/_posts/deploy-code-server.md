---
title: 部署 Code-Server，随时随地 VsCode
categories:
  - Programing
tags:
  - Code-Server
date: 2021-06-05 10:22:20
---
[Code-Server](https://github.com/cdr/code-server) 是一个可以在远程服务器上运行 VS Code 的工具。拥有与 VS Code 完全相同的界面和使用方法。

![](https://gitee.com/whwtf/upic/raw/master/uPic/codee.jpeg)

将其部署在远程服务器上，就可以通过 Edge、Chrome 等浏览器在任何设备上运用完全一致的环境进行写作与开发，一切计算都在服务器上进行，不用考虑终端设备的性能。

在[Releases](https://github.com/cdr/code-server/releases)页面找到相应的 rpm 或者 deb 软件包下载并上传到远程服务器（也可以直接在服务器上使用 wget 等工具直接下载）并安装。

然后，创建 ～/.config/code-server/config.yaml 文件，内容如下：


```
bind-addr: 0.0.0.0:8999        #远程访问的端口
auth: password
password: xxxxxxxxx            #访问时登陆的密码
cert: true                     #启用https，不然有些功能限制使用
```

编辑 /lib/systemd/system/code-server@.service 文件，内容修改如下：


```
[Unit]
Description=code-server
After=network.target

[Service]
Type=exec
ExecStart=/usr/bin/code-server --cert 公钥 --cert-key 私钥
ExecReload=/usr/bin/code-server --cert 公钥 --cert-key 私钥
Restart=always
User=%i

[Install]
WantedBy=default.target
```

保存退出后，执行如下命令：


```
sudo systemctl enable --now code-server@username    #username替换成当前登陆用户名
```

之后，就可以在浏览器中使用 https://服务器ip:设定的端口 的形式来访问 code-server 了。