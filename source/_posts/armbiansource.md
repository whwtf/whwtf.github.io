---
title: Armbian 安装并改为国内软件源
date: 2021-04-14 11:17:54
categories:
- Linux
- SomePi
tags:
- Armbian
---
Armbian是 [@armbian.com][armbian]] 专为RaspberryPi、NanaoPi、RockPi……这类使用arm soc的微小计算机/开发板编译制作的操作系统。运行稳定，功能强大。内置armbian-config配置程序，可以方便的对系统进行设置和安装一些第三方软件。
![](https://gitee.com/whwtf/upic/raw/master/uPic/armbian-config2020-09-25.png)
![](https://gitee.com/whwtf/upic/raw/master/uPic/soft2020-09-25.png)

## Armbian安装
从 [armbian官网][armbian] 下载对应的系统镜像文件，用烧卡软件写入到TF卡中。个人认为 **[Etcher][etcher]** 比较好用。
![](https://gitee.com/whwtf/upic/raw/master/uPic/etcher2020-09-25.png)
插入卡，通电，等待几分钟就可以SSH连接了。第一次用 root@1234 登录，首次进入系统需要更改root密码，新建用户。然后重启，用新建的用户登录，使用命令
```
$ sudo armbian-config
```
对系统进行设置和按需安装第三方软件。

## 更改apt源为国内
[Armbian][armbian] 默认软件源为 [Debian][debian] 官方的，使用起来速度比较慢，可以更改为国内源加快更新及安装速度。
国内Linux源有很多，通常使用 [清华大学][tsinghua] 的，有详细的使用文档，还有各种系统的国内下载镜像，很方便。
首先：
```
$ sudo apt install apt-transport-https ca-certificates
```

[Armbian][armbian] 更改源的时候需要改两个地方：
```
$ sudo nano /etc/apt/sources.list
```
将里面内容全部注释掉，添加：
```
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释，“buster”根据版本更改。
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ buster main contrib non-free
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ buster main contrib non-free
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ buster-updates main contrib non-free
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ buster-updates main contrib non-free
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ buster-backports main contrib non-free
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ buster-backports main contrib non-free
deb https://mirrors.tuna.tsinghua.edu.cn/debian-security buster/updates main contrib non-free
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian-security buster/updates main contrib non-free
```
然后还有一个地方需要更改，是Armbian自身的内容更新源
```
$ sudo nano /etc/apt/sources.list.d/armbian.list
```
注释掉原来内容，添加：
```
“buster”根据版本更改
deb http://mirrors.tuna.tsinghua.edu.cn/armbian/ buster main buster-utils buster-desktop
```

之后就可以愉快的
```
$ sudo apt update
$ sudo apt upgrade
or
$ sudo apt dist-upgrade
```



[armbian]:https://www.armbian.com/
[etcher]:https://www.balena.io/etcher
[debian]:https://www.debian.org/
[tsinghua]:https://mirrors.tuna.tsinghua.edu.cn/