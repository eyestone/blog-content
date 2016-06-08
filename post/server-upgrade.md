+++
categories = ["server"]
date = "2016-06-03T09:21:36+08:00"
draft = false 
tags = ["java", "spring"]
title = "服务器代码的进化史"
author = "freeopen"

+++

代码之美如同数学之美，好的代码通常是结构清晰，简单优雅的。
代码如同生命，需要时间的磨砺和进化，下面将对我们的服务器代码需进化的方向做系列分析。

#### **1. 运行零配置**

一般服务器代码需要部署到容器之中才能正常运行，这不太符合拿来即用的原则。本地环境复杂化一向不是我推崇的方法，中国人自有最优秀的设计基因，比如中国的筷子就是比老外的刀叉更优秀的设计，所以复杂的代码系统不仅仅是功能设计，还需要优秀的配置管理。

**已搞定**

目前，通过对gradle脚本的改造，已经实现“运行零配置”的目标，方法为：

~~~
cd welfare
gradle tomcatRunWar
~~~

你可以说，上面的代码还不完全是零配置，还需要安装gradle才能运行，当然不是：

~~~
cd welfare
../gradlew tomcatRunWar 
~~~

gradlew 命令是在project根目录下的命令行，会自动在线下载gradle的运行环境，当然第一次执行时因为下载会比较费时。

**待解决的问题**

- 代码中缓存系统memcached关闭后，tomcat启动时仍在不停的连接12345端口，这意味着代码中memcached的连接存在不能关闭的隐患

--- 

#### **2. session共享**

**待解决**

session通常记录用户的登录信息，保持用户的登录状态。

现在的session系统采用spring默认的http session，只能自己用，不能分享给别人用，这种情况不利于未来微架构系统的分布式部署。

**解决方案**

采用spring-session框架, 将session由内存数据库redis接管。

---

####  **3. 紧耦合的session和权限管理**

**待解决**

为实现标题中的功能，把代码写得到处都是，代价是不易维护，增加各种潜在隐患。

**解决方案**

采用切面技术将session和权限功能进行接管。

---

#### **4. 丑陋的memcached**

**待解决**

先声明，其实memcached作为一个缓存技术，一点也不丑陋，这里的丑陋是指我们的使用它的代码丑陋。我们的使用方法是先写了一个数据库工具框架，然后在框架中嵌入memcached调用，要对那个表数据缓存，就把该表增加到insert，update，delete三个基本操作中。在很多年前，这样的设计是最优秀的方法之一，但时至今日，这样的设计不够灵活，现在增加一个缓存可以到方法上，简单到增加一个注解即可。

其二，对于缓存系统的使用，我更倾向于被广泛使用且兼容性好的。比如现在redis的被支持率明显高于memcached，很多框架代码缺省支持redis，如果我们想继续使用memcached，必须自行寻找各种支持库，麻烦费时。

**解决方案**

用redis代替memcached，但由于涉及到数据库处理层的修改，需要保持极度谨慎。

---

#### **5. 多环境零配置**

**待解决**

现有的代码环境，简单分为开发环境和生产环境，而开发环境指向的域名为laifucard.net,生产环境为.com，在gradle编译时只需要通过snapshot和release参数就能区分产生不同环境的运行时war文件。

但，实际代码的实现方式却不优雅。仅仅是为了在不同编译参数时采用不同的域名，居然在service子项目中写了两个bean，同时还在两个xml文件中分别进行配置，想想就心塞。

**解决方案**

- 将代码环境分为 开发环境 生产环境 和 本地开发环境，因为目前的开发环境主要适用于laifucard.net环境
- 环境变量写到文本配置文件中，目前主要包括域名配置和数据库配置信息
- 由程序自动选择性读取不同的环境变量，可采用spring的profile特性
- gradle脚本增加一个编译选项，命名为local

---

#### **6. 自动化测试**

**待解决**

目前代码并没有养成单元测试的习惯，服务器端的单元测试大量集中于对接口的测试，实现技术主要采用google的mokito框架。

**解决方案**

- 针对已有接口写单元测试模板，供大家参考
- gradle脚本增加集成测试编译选项，使其自动化

---

#### **7. 代码检查**

**待解决**

目前的代码检查方案是checkstyle,我原来已经做过检查。问题在于该工具的检查深度不够，我们需要更高级的工具帮我们来改善代码质量.

**解决方案**

- 启用sonarqube技术，对源码做最全面的检查
- 根据检查结果，消除隐患，清除代码冗余

---

#### **8. 异步消息系统**

**待解决**

服务器系统中有短信通知、邮件通知、app通知等多种通知模式，在现阶段没什么用户的情况下问题不大，但当用户基数上去以后，最好将通知系统由异步消息系统接管，才能进一步提高服务器效率。

所谓异步消息系统就好比一个邮局，服务器程序只需要把通知内容交给邮局，就可以忙自己的其他事情了，剩下的事情都交给邮局自行处理。

**解决方案**

- 服务器端部署消息代理系统
- 改造msg-push子项目，进行xml文件配置即可

---

(未完，待续)