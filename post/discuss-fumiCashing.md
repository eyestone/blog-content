+++
categories = ["讨论"]
date = "2016-05-31T14:38:51+08:00"
tags = ["福米兑换场", "订单"]
title = "福米兑换场流程和相关接口的讨论"

+++

#### 问题提出

- 对于即将开展的工作内容的常规讨论

#### 讨论结果

- 1.流程图如下，图中备注为需要用到的接口，打钩的表示已经实现的

<img src="https://raw.githubusercontent.com/eyestone/eyestone.github.io/master/img/fumiExchangeFlowChart.png" width = "550"  alt="福米兑换场流程" align=center />

- 2.在旧版本的代码中实现相关接口
- 3.使用spring web flow技术实现订单部分内容
- 4.对数据库表进行了相应的修改
- 5.order_status表中增加1个状态：已处理，表示我们平台已经将信息提交到供应商。  

 -  name　　　　value　　　　remark  
 - 已删除　　　　0    
 - 未付款　　　　1
 - 已付款　　　　2
 - 已取消　　　　3
 - 已消费　　　　4
 - 申请退款中　　5
 - 已退款　　　　6
 - 已评价　　　　7
 - 已处理　　　　8　　　　　　表示我们平台已经将订单通知到供应商，可能需要改编号

- 6.order_v2表中增加一个exception_code字段，用于记录异常信息。
 - name　　　　value　　　　remark
 - 正常　　　　　0
 - 订单缺货　　　1

- 7.订单过程，因为目前没有办法监控供应商和物流，所以对于我们来说，用户付款就认为交易完成。

#### 数据库相关

- 1.购物车：bills，charge_record，collection，service_invoke_record
- 2.订单：order_pay(暂时不用)，order_detail，link_type，order_v2(在用)，order(旧表，不建议用)，order_status，order_ticket_link，user_delivery_info
- 3.merchandise_off_price(不建议用，在相关的表中使用tags字段作为另一个价格)，merchandise_specification，merchandise_type，merchandise_qualification，merchandise

#### TODO

- 1.增加收货地址表，设置：userUuid，address，consignee，default，modifyTime字段
- 2.实现相关接口

#### 问题

- 1.考虑多商品订单如果往新的表（分子表）中存放，会出现什么问题？

#### 参与人

- 林欣灏
- 陆文金

#### 记录

- 陆文金
