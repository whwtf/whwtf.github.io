---
title: Linux 终端增强（通过 zsh 与 oh-my-zsh）
categories:
  - null
tags:
  - null
date: 2021-04-15 11:45:12
---

本文适用于 Debian 系 Linux，MacOS 同样可以。  
首先安装 [zsh](https://www.zsh.org/) （[这里](https://github.com/ohmyzsh/ohmyzsh/wiki/Installing-ZSH)有详细说明）：

```
sudo apt update
sudo apt install zsh
```

然后，将 zsh 设置为系统默认终端：

```
chsh -s $(which zsh)
```

接下来安装 [oh-my-zsh](https://raw.github.com/ohmyzsh/ohmyzsh/)，这是 zsh 的终极扩展和配置工具：

```
sh -c "$(wget https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)"
```

oh-my-zsh 的美丽外观得益于其自带的许多主题，但是其中很多需要 [Poweline-fonts](https://github.com/powerline/fonts)，所以海需要安装：

```
sudo apt install fonts-powerline
```

现在可以通过修改 on-my-zsh 的配置文件来选择一个喜欢的主题了（[这里](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes)有内置所有主题的预览）：

```
nano ~/.zshrc
```

找到主题定义这一行，是这样的：

```
ZSH_THEME="agnoster"  # agnoster是个人喜欢的
```

oh-my-zsh 同样支持非常多的插件来扩展功能，同样在. zshrc 配置文件中：

```
nano ~/.zshrc
```

找到：

```
plugins=(git)  # 默认只启用了git插件
```

[这里](https://github.com/ohmyzsh/ohmyzsh/wiki/Plugins)有所有插件列表及介绍。  
还有一些功能插件不是内置的，需要手动安装。  
1、zsh-syntax-highlighting，让 zsh 拥有了语法高亮功能

```
sudo apt install zsh-syntax-highlighting
```

然后修改配置文件，在末尾增加一行：

```
source /usr/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```

2、zsh-autosuggestions，记录曾经输入过的命令，方便重复输入：

```
git clone https://github.com/zsh-users/zsh-autosuggestions.git
sudo cp -r zsh-autosuggestions /usr/share/
```

修改配置文件，在末尾添加：

```
source /usr/share/zsh-autosuggestions/zsh-autosuggestions.zsh
```

大功告成，现在退出终端，注销用户然后重新登录，就可以打开增强之后的终端了。
