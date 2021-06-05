---
title: 用 Pipenv 建立与管理 Python 虚拟环境
categories:
  - Programing
  - Python
tags:
  - Python
  - Pipenv
date: 2021-04-15 11:32:59
---

[](##一、什么是-pipenv "一、什么是 pipenv")一、什么是 pipenv
--------------------------------------------

pipenv 主要包含了 Pipfile、pip、requests 和 virtualenv。Pipfile 是新型依赖管理文件，用于替代过于简陋的 requirements.txt 文件。  
在 pipenv 管理下，一个项目对应一个 Pipfile，包含项目所使用的环境，依赖名称及版本，支持开发环境与正式环境区分。默认提供 default 和 development 区分。用 Pipfile.lock 提供版本锁定支持 。

[](##二、使用方法 "二、使用方法")二、使用方法
--------------------------

*   安装
    
    ```
    $pip3 install pipenv
    #python3
    ```
    
*   环境创建  
    新建一个存放当环境的文件夹 project1，并进入该文件夹
    
    ```
    $pipenv --three
    #使用当前系统的Python3创建环境
    ##$pipenv --python 3.6
    #指定某一Python版本创建环境
    ```
    
    这会在当前文件夹建立一个 Pipfile，指定 pip 源，创建环境解释器及依赖包的位置。
    
*   安装依赖包
    
    ```
    $pipenv install django
    ##$pipenv install --dev django        #安装依赖包，但只在在Dev环境中关联
    ##$pipenv install django==1.11        #指定版本安装
    ```
    
    安装依赖包放入环境文件夹并记录在 pipfile 中
    
*   卸载依赖包
    
    ```
    $pipenv uninstall django
    ##$pipenv uninstall --all        #卸载所有已安装包
    ```
    
*   运行程序 有两种方式运行程序  
    1、直接运行
    
    ```
    $pipenv run python3 project1.py
    ```
    
    2、启动虚拟环境运行
    
    ```
    $pipenv shell      #启动虚拟环境
    $python3 project1.py
    $exit              #退出虚拟环境
    ```
    
*   重建环境 在新设备中，通过 git clong 了项目文件并经过 pipenv 初始化后，可以执行：
    
    ```
    $pipenv install
    ##$pipenv install -dev        #安装所有依赖包，包含Dev版本
    ```
    
    来重建虚拟环境，pipenv 会根据文件夹中的 pipfile 自动选择 Python 版本，依赖包的安装及版本。
    

[](##三、其他功能 "三、其他功能")三、其他功能
--------------------------

```
$pipenv --where        #显示目录信息
$pipenv --venv         #显示虚拟环境信息
$pipenv --py           #显示Python解释器信息
$pipenv graph          #查看目前安装的库及其版本
$pipenv check          #检查安全漏洞
```

*   使用 pipenv install 安装依赖包时默认用的国外源，速度慢，可改成国内源。 修改 pipfile 中 url 参数的值，比如改成以下其中一条：
    
    ```
    url = "https://pypi.tuna.tsinghua.edu.cn/simple"
    url = "http://mirrors.aliyun.com/pypi/simple/"
    url = "https://pypi.mirrors.ustc.edu.cn/simple/"
    url = "http://pypi.douban.com/simple/"
    url = "https://pypi.tuna.tsinghua.edu.cn/simple/"
    url = "http://pypi.mirrors.ustc.edu.cn/simple/"
    ```
