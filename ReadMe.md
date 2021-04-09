## 1、安装git
```
sudo apt-get install git
```

## 2、设置git全局邮箱和用户名
```
git config --global user.name "yourgithubname"
git config --global user.email "yourgithubemail"
```

## 3、设置ssh key
```
ssh-keygen -t rsa -C "youremail"
#生成后填到github上
#验证是否成功
ssh -T git@github.com
```

## 4、安装nodejs
```
sudo apt-get install nodejs
sudo apt-get install npm
```

## 5、安装hexo
```
sudo npm install hexo-cli -g
```

但是已经不需要初始化了，直接在任意文件夹下：
```
git clone git@………………
```
然后进入克隆到的文件夹：
```
cd xxx.github.io
npm install
npm install hexo-deployer-git --save
```

## 6、安装插件

### [hexo-wordcount](https://github.com/willin/hexo-wordcount)

```
npm install hexo-wordcount --save
```
### [hexo-generator-json-content](https://github.com/alexbruno/hexo-generator-json-content)

```
npm install hexo-generator-json-content --save
```
### [hexo-generator-feed](https://github.com/hexojs/hexo-generator-feed)

```
npm install hexo-generator-feed --save
```
### [hexo-generator-sitemap](https://github.com/hexojs/hexo-generator-sitemap)

```
npm install hexo-generator-sitemap --save
```
### [hexo-generator-baidu-sitemap](https://github.com/coneycode/hexo-generator-baidu-sitemap)

```
npm install hexo-generator-baidu-sitemap --save
```

## 7、生成，部署：

```
hexo g
hexo d
```

然后就可以开始写你的新博客了
```
hexo new newpage
```

## 8、每次写完最好都把源文件上传一下
```
git add .
git commit –m "xxxx"
git push 
```

如果是在已经编辑过的电脑上，已经有clone文件夹了，那么，每次只要和远端同步一下就行了
```
git pull
```
