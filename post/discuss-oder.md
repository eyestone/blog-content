+++
categories = ["讨论"]
date = "2016-06-06T17:23:24+08:00"
tags = ["订单", "商品"]
title = "订单和商品部分底层设计"

+++

#### 问题提出

- 如何从商品信息中获取order_process表中的标识位的相关信息
- 如何向用户展示详细的商品信息

#### 讨论结果

- 1.商品种类和卖法的总结如下图：

<img src="https://raw.githubusercontent.com/eyestone/eyestone.github.io/master/img/merchandise.png" width = "550"  alt="福米兑换场流程"       align=center />

- 2.因为卖法涉及到价格计算以及一些业务流程的变化，而一个商品可能涉及到多种卖法，所以讲卖法单独拿出来作为一个表，其中要包括卖法相关的信息和价格。
- 3.目前订单部分为了照顾之前商品部分的内容，暂时不处理order_process表中的标识字段。
- 4.在第一阶段的order部分内容开发完成之后开始重构商品部分，同时完善订单部分内容。

#### 参与人

- 林欣灏
- 陆文金

#### 记录

- 陆文金

