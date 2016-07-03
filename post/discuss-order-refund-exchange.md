+++
categories = ["设计", "讨论"]
date = "2016-07-03T15:09:27+08:00"
tags = ["订单", "退换货"]
title = "关于订单和退换货重新设计的讨论"

+++

#### tc：一个馒头引发的血案 之 一个status引发的重构

### 设计原则

- 将各个业务模块分离开，保证业务逻辑的独立性
- 统一各个表的id，使用java写一个long型的uuid生成算法

### 订单

#### 1.数据库表修改

- 1.1 order_v2表添加如下字段
 - is_deleted: boolean 取消后和收到货物后可删除，逻辑删除
 - is_canncelled: boolean 未付款之前可以取消
 - is_payed: boolean 是否支付
 - is_refunding: boolean 是否正在退款，用于表示此订单中是否有商品正在退款
 - has_refund_record: boolean 是否有过退款记录
 - user_receipt: boolean 用户确认收货
 - gmt_user_receipt: DateTime 用户确认收货时间
 - order_status: int 0新生成，1已检查，2提交给供应商，3已发货，4确认收货
 - gmt_status: string json字符串，用于存储每一个状态发生的时间，每发生一次状态改变增加一个记录
 - gmt_expiration: DateTime 订单过期时间，到时间未支付的，过期。如果这个字段为空，表示不过期

- 1.2 order_detail表添加如下字段
 - is_used: boolean 是否以使用，对于虚拟商品有效 

- 1.3 delivery_method表
 - 添加一个福米价

#### 2.业务过程梳理

- 2.1 确认收货相关
 - 订单提交给供应商以后10天自动将状态修改为已收货
 - 自动处理的，同时将用户收货状态和系统收货状态置为已收货
 - 用户确认收货以后，系统收货状态同时变成已收货
- 2.2 分单
 - 在后台订单处理系统中，将订单按照商家和用户分单
- 2.3 ui
 - 在订单上显示：已支付，未支付状态，如果有正在退款的商品，显示退款中，不显示退款完成状态
 - 在订单上增加“确认收货”按钮，在全部商品都收到的情况下才点，也就是说一旦确认了，就认为所有商品都收到了
 - 在订单商品中增加退款和换货状态
- 2.4 其他
 - 对于一些有特殊处理要求的商品，不能和其他商品进入同一个订单，比如有支付截止时间的等等

#### 3.需要改动的代码量

- 3.1 订单生成时保存订单状态，直接生成和从购物车生成，合并两个函数的代码 -- 1小时
- 3.2 订单支付完成时修改订单状态 -- 2小时，连同之前耦合的支付相关代码一并处理
- 3.3 订单生成时取福米价 -- 10分钟


### 退换货

#### 1.数据库表修改

- 1.1 refund_apply
 - 去掉refund_detail字段
 - 增加refund_no: 退款单号
- 1.2 增加refund_detail表
- 1.3 refund_detail
 - uuid: 
 - refund_apply_uuid
 - gmt_create: datetime 创建时间
 - gmt_update: datetime 修改时间
 - gmt_refund: datetime 退款时间
 - our_remark: string 我们的注释
 - order_detail_id：
 - delivery_fee_to_user: 寄给用户的运费，换货的时候使用
 - to_user_payer: 寄给用户的运费支付方，0表示用户，1表示商家，2表示来福
 - delivery_fee_to_busi: 寄给商家的运费，退换货都要用
 - to_busi_payer: 寄给商家的运费支付方，同上。注意的是，这里记录的是这笔钱应该由谁出，而不是实际由谁出，如果涉及到要垫付的情况，需要由运营人员自行记录处理。
 
 

#### 2.业务过程梳理

- 2.1 退货和换货都保存到这两个表中
- 2.2 用户点击退换货按钮，填写原因，上传照片；运营人员电话联系，确认要退换的货物的详情；后台生成退款单；
- 2.3 换货由用户和运营人员私下交流决定

#### 3.需要修改的代码量

- 3.1 修改保存退换货函数 -- 1.5小时

### 卖法

#### 1.数据库修改

- 1.1 增加一个卖法表sold_method，包括如下字段
 - uuid: long
 - name : string 卖法名称
 - source_price : decimal(9,2) 原始价格
 - sold_price : decimal(9,2) 卖价
 - fumi_price : decimal(9,2) 福米价
 - remark : string 备注
 - status : int 0表示下架，1表示有效
- 1.2 增加一个计划表sold_sequence，包括如下字段
 - uuid : long
 - merchandise_uuid: string 
 - sold_method_uuid: long
 - is_default: boolean 是否默认，如果在对应的时间内，没有卖法的，都使用这个卖法
 - start_time: datetime 开始时间
 - end_time: datetime 结束时间
 - status: int 0表示下架，1表示有效，2表示还未到
- 1.3 merchandise表增加一个卖法id
 - sold_method : long 

#### 2.业务过程梳理

- 2.1 当对应的时间段有卖法的时候，使用对应的卖法，否则使用默认的卖法


#### 3.需要修改的代码

- 3.1 管理端上商品，添加卖法和卖法时序
- 3.2 取商品的时候，到计划表和卖法表中取
- 3.3 生成订单接口函数中取价格获取问题，写一个取价格函数，传入订单的生成时间和商品id，以及其他一些相关参数，来检索价格

