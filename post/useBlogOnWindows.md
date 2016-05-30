+++
categories = ["使用说明", "windows"]
date = "2016-05-25T11:45:30+08:00"
tags = ["blog", "windows"]
title = "在windows下使用hugo写博客"

+++

- 运行框（开始按钮旁边的小框框）输入cmd，回车打开命令行工具
- 输入cd D: 回车  这个是跳转到相关的磁盘，如果是E盘，就输入E:
- 输入 D: 回车  这个是切换到相应的盘
- 输入 cd dir 回车  dir为要进入的目录，比如你的blog目录放在D:/blog就直接输入 cd blog
- 新建文件 hugo new post/filename.md 这样就会在blog/content/post目录下创建一个filename.md文件
- 用文本编辑器打开要编辑的文件进行编辑保存
- 在命令行输入show.cmd，然后在浏览器输入localhost:1313即可查看本地的文档
- 使用deploy.cmd命令，可以将本地文档同步到服务器
