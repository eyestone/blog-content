+++
categories = ["约定", "规范"]
date = "2016-07-20T14:08:46+08:00"
tags = ["约定", "规范"]
title = "工作规范和流程"

+++

#### 原则
- 够用 + 最小化
- 方便他人，送人玫瑰手留余香
- 有问必有答

#### 邮箱
- 重要的事情需要从邮件进行沟通
- 收到邮件务必在第一时间回复
- 每天至少查看n次邮箱，n>0

#### github博客
- 用于讨论确定事宜和工作规范文档
- 时常查看


#### github *
- 用于代码管理和任务管理
- github代码管理
 - 有重要修改的时候必须提交一次代码
 - 原则上每天必须提交一次代码
 - 其他的协商决定提交
- github任务管理
 - 1.充分分析依赖资源，主要是执行人和前置任务
 - 2.将任务以issue的形式发布到github：描述任务尽量详细，避免任务接收者还要再来询问；描述清楚验收标准；如果是其他任务的前置任务一并说明；
 - 3.相关人员接受任务后进行comment回复，以便确认任务处于何种状态
 - 4.如果开发过程中遇到问题，及时在相关issue下说明具体情况，给出自己的分析结果和可能造成任务延时情况，并@相关人员
 - 5.任务完成后在issue下面说明完成的情况，注明是否会有潜在的问题，然后关闭任务
- github任务模板
 - 标题:

```
 模块 - 标题内容
 模块：ios、android、server、project ...
 举例：server - 修改默认地址删除逻辑
 ```
  - 内容：

 ```
 关键字
 exec：执行人，参与人，一般将首要执行人写在最前面
 confirm：任务确认和关闭人
 deadline：截止时间
 tasks：任务详情
 comment：备注

 举例：
 project - 整理项目总结资料

 exec: 陆文金
 confirm: 林总 & 陆文金
 deadline：2016-07-24 22:00:00
 tasks:
 1.整理代码风格和注释说明
 2.在showdoc上建立android和ios目录，并写范文和要求
 3.修改github使用规范文档
 4.整理培训的方案文档
 5.整理需要使用到的工具和技术列表

 comment：
 5会在总结会之后给出
 此任务需要在项目总结会之前完成
 ```
  - 总结

 ```
 任务已完成
 1.见showdoc上相关项目的“基本原则”目录下的编码规范，ios阅读readme
 2.同1
 3.见eyestone.github.io上的《工作规范和流程》
 4.同3
 5.在总结会之后给出
 注：文档大多是例子，需要不断修改进化
 ```

#### 培训
 - 主题模式，收集问题，针对问题培训
 - 设计场景，动手操作
 - 抽象的东西一起培训，具体的东西单独培训
 - 培训后需要由被培训人出文档，将培训的内容整理清楚作为正式文档保存

#### 建议
- 1.讨论
 - 1.1.提出问题
 - 1.2.总结思考确定是否确实需要讨论
 - 1.3.整理讨论的点
 - 1.4.组织相关人员讨论
 - 1.5.讨论结果形成文字，发布到相关位置
 - 1.6.邮件通知相关人员查看
 - 1.7.有疑问则继续讨论知道定稿
 
- 2.会议
 - 2.1.会前每个人将要提的问题准备一份书面报告，回答以下问题：
 
 ```
 究竟出了什么问题?
 问题的起因是什么？
 这个问题能找到哪些解决方案？
 你自己建议用哪一种方法？
 ```



