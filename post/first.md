+++
categories = ["教程"]
date = "2016-05-18T21:20:54+08:00"
tags = ["blog"]
title = "github博客使用说明"

+++

### 1.概述

本博客系统由hugo（go语言本地博客系统）结合github pages组成，采用markdown格式编辑，用hugo命令自动生成静态网页，采用git命令行进行发布。

善用本系统可获得如下好处，一个是本地编辑好过网页编辑，特别是网站很慢网络不稳定的时候; 另一个是markdown 格式的编写，可以让作者不再关注鼠标式的排版问题，真正聚焦于内容创作上。


博客访问地址为`eyestone.github.io`

### 2.系统安装

**hugo 安装**

[hugo](https://github.com/spf13/hugo/releases),根据上述链接下载自己操作系统匹配的版本，如windows下载hugo_0.15_windows_amd64.zip，mac系统`brew install hugo`, windows环境下解压文件后，建议将hugo命令所在目录设置到系统环境变量。

**git 安装（略）**

**blog 模版安装**

将公司共享服务器中blog目录复制到本机

**第一次使用时，克隆远程库**

```
$ cd blog
$ git clone https://github.com/eyestone/eyestone.github.io.git public 
```

### 3.用markdown语法撰写博客

**在blog目录下，运行如下命令行**

``` 
$ hugo new post/filename.md  //将在content/post/目录下自动生成blog模板文件
```

**模版文件格式如下**

```
+++
title         = "文章标题"
tags         = ["标签1", "标签2"]
categories    = ["分类1","分类2"]
date        = "2015-01-01"
+++

你的正文
```
这时，使用任意的文本编辑器即可书写博客了。

**博客预览**

blog目录下运行
```
$ ./show.sh   // mac or linux 平台

$ show      // windows 平台
```
打开网页浏览器，输入地址

```
localhost:1313
```

**发布到github**

- 手动发布(当前目录为blog 目录)

```
$ git pull   //养成习惯，先更新库
$ hugo
$ cd public
$ git add -A
$ git commit -m "add file title"
$ git push
```

- 自动发布

```
$ ./deploy.sh
```

进阶，有兴趣者也可以考虑自行写windows批处理文件.


### 4.One more thing

时时跟踪文章更新，才是博客系统最有力的功能。
写文章的同学，每次写完，还要邮件通知其它同事，是不是很麻烦？
下面仅对苹果手机做简单说明。

* 官方浏览器打开本博客地址
* 浏览器最下方选择 分享 图标，再选择 @ 图标
* 再到浏览器的书签图标中点击“订阅”

android手机的订阅说明，请其它有兴趣的同学继续补充。
