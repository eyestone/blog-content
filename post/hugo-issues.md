+++
categories = ["hugo" ]
date = "2016-05-31T10:21:34+08:00"
tags = ["hugo", "markdown]
title = "关于hugo中markdown格式的兼容性问题"

+++

目前hugo并不兼容markdown的高级格式功能，比如todo list和流程图。

经检查源码，发现是markdown的解码问题，原理为解析markdown格式文件为html格式文件，特别是流程图功能，用到了html5的矢量图特性。这类的功能增强要么我们自己扩展，要么坐等官方更新。

仅作记录，空了继续补充。
