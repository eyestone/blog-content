+++
categories = ["设计"]
date = "2016-07-03T14:59:31+08:00"
draft = false 
tags = ["支付", "签证", "活动"]
title = "支付及签证、活动设计"

+++

### 支付原则

- 独立模块，只从订单获取基本支付信息，不同的支付方式存入不同的表格

### 支付接口及参数

- Class Name : AnyPayV2
- Method Name : PayRequest 
    - 和订单表们和支付表 耦合 
    - 支付和退款前的参数封装
    - 将支付信息写进对应的支付表中
    - from：1= 福米兑换场, 2 = Visa, 3 = Activity  
    - orderId: bigInt(20)
    - isRefund: true, false
    - payMethod: weixin, alipay, fumi
    - return: 针对不同支付封装好参数Map
    - 注：支付宝直接返回参数map，微信需调用一次回调才能返回

- Method Name : payNotify
    - 回调函数，和支付表耦合
    - 获取回调通知
    - 将回调信息补充进对应的支付表中
    - 兼容 支付 和 退款
    - return: rtn_code, rtn_msg 若失败则在rtn_msg中写入错误信息 
    - 注：微信有 2 次回调

- Method Name : payAfter, 支付完成后处理
    - 和订单表们耦合
    - 根据订单来源进行支付后数据处理

### 支付宝交互逻辑及表结构

#### 支付宝交互流程图

[doc](https://doc.open.alipay.com/doc2/detail.htm?spm=a219a.7629140.0.0.DWEKRM&treeId=59&articleId=103658&docType=1)

#### 1. 支付请求

- [doc](https://doc.open.alipay.com/doc2/detail?articleId=103663&docType=1&treeId=59) 
- 由手机端sdk发起请求，服务器端生成请求数据包 

#### 2. 异步通知

- 服务器获取支付宝服务器的异步通知
- 更改服务器支付数据

- [doc](https://doc.open.alipay.com/doc2/detail.htm?spm=a219a.7629140.0.0.mKs6tp&treeId=59&articleId=103666&docType=1)

#### 3. 获取支付结果

- 手机端从支付宝服务器获取支付结果
- 将支付结果展示给用户

#### 表结构

- order_id
- from 
- type  pay or refund 

- notify_time	通知时间	Date yyyy-MM-dd HH:mm:ss
- notify_type	通知类型	String
- notify_id	通知校验ID	String
- sign_type	签名方式	String
- sign	签名	String
- out_trade_no	商户网站唯一订单号	String(64)
- subject	商品名称	String(128)
- trade_no	支付宝交易号	String(64)
- seller_id	卖家支付宝用户号	String(30)
- seller_email	卖家支付宝账号	String(100)
- buyer_id	买家支付宝用户号	String(30)
- buyer_email	买家支付宝账号	String(100)
- total_fee	交易金额	Number 1yuan
- trade_status	交易状态	String 
- refund_status	退款状态	String
- gmt_refund	退款时间	Date
- gmt_payment	交易付款时间	Date
- use_coupon	是否使用红包买家	String(1)
- discount	折扣	String 如有，负数，单位元

通知交易状态 trade_status

- WAIT_BUYER_PAY	交易创建，等待买家付款。
- TRADE_CLOSED	在指定时间段内未支付时关闭的交易；在交易完成全额退款成功时关闭的交易。
- TRADE_SUCCESS	交易成功，且可对该交易做操作，如：多级分润、退款等。
- TRADE_FINISHED	交易成功且结束，即不可再做任何操作。

退款状态 refund_status

- REFUND_SUCCESS 退款成功。全额退款情况：trade_status=TRADE_CLOSED，而refund_status=REFUND_SUCCESS；
- REFUND_CLOSED 退款关闭

### 微信支付交互逻辑及表结构

#### 微信交互流程图

[doc](https://pay.weixin.qq.com/wiki/doc/api/app/app.php?chapter=8_3)

#### 1. 生成预支付单

商户系统先调用该接口在微信支付服务后台生成预支付交易单，返回正确的预支付交易回话标识后再在APP里面调起支付，详见
[官方文档](https://pay.weixin.qq.com/wiki/doc/api/app/app.php?chapter=9_1)

手机端发起支付请求， 服务器生成预支付单，返回手机prepay_id 和 sign等

#### 2. App调用支付接口 

app 收到 服务器 传来的 预支付交易会话id和微信返回的签名后，才能发起支付调用，
详见[官方文档](https://pay.weixin.qq.com/wiki/doc/api/app/app.php?chapter=9_12&index=2)
第一次回调

#### 3. 服务器获取支付结果通知

- [官方文档](https://pay.weixin.qq.com/wiki/doc/api/app/app.php?chapter=9_7&index=3)
- 当微信服务器没有收到我方服务应答时，按该时间频率重发通知（通知频率为15/15/30/180/1800/1800/1800/1800/3600，单位：秒）
- 微信通知从 notify_url 处接受，必须保证该 url 线上可访问
- 先从微信的通知数据获得如下信息
- return_code : success 通信成功标识,然后有一大堆返回值
- result_code : success 交易成功标识
- 接收完通知后，我方服务器再向微信服务器进行应答，返回 return_code: success, 有问题时，在return_msg 中填写错误信息
- 服务器进行支付后的数据处理
- 第二次回调

#### 4. 完成交易

- 手机端向服务器端查询支付结果
- 服务器调用api向微信服务器查询支付结果
- 服务器收到支付成功结构后，向手机端返回支付结果
- 手机端向用户展示支付结果

#### 5. 关闭订单

- [官方文档](https://pay.weixin.qq.com/wiki/doc/api/app/app.php?chapter=9_3&index=5)
- 以下情况需要调用关单接口：商户订单支付失败需要生成新单号重新发起支付，要对原订单号调用关单，避免重复支付；系统下单后，用户支付超时，系统退出不再受理，避免用户继续，请调用关单接口。
注意：订单生成后不能马上调用关单接口，最短调用时间间隔为5分钟。

#### 5. 申请退款

- [官方文档](https://pay.weixin.qq.com/wiki/doc/api/app/app.php?chapter=9_4&index=6)
- 交易时间超过一年的订单无法提交退款；
- 微信支付退款支持单笔交易分多次退款，多次退款需要提交原支付订单的商户订单号和设置不同的退款单号。一笔退款失败后重新提交，要采用原来的退款单号。总退款金额不能超过用户实际支付金额。
- 需要双向证书
- 提交退款申请后，通过调用该接口查询退款状态。退款有一定延时，用零钱支付的退款20分钟内到账，银行卡支付的退款3个工作日后重新查询退款状态。

#### 6. 退款查询

- 退款有延时，用来判断是否退款成功
- [官方文档](https://pay.weixin.qq.com/wiki/doc/api/app/app.php?chapter=9_5&index=7)


#### 表结构设计

字段内容为接口调用的参数，错误状态记录，每种情况的时间记录，和支付信息概要

- order_id
- from 
- type  预付单，预付单返回， 支付，支付返回，退款，退款返回，关闭，关闭返回

- 应用ID	appid 32 (const)
- 商户号	mch_id 32 (const)
- 随机字符串	nonce_str 32
- 签名	sign 32
- 商品描述	body 128
- 商户订单号	out_trade_no 32 (from order_id)
- 总金额	total_fee Int 单位 分
- 终端IP	spbill_create_ip 16	
- 交易起始时间	time_start 14
- 交易过期时间	time_expire 14 交易过期时间
- 通知地址	notify_url 256 (const)
- 交易类型	trade_type	16  
- 返回状态码	return_code 16
- 返回信息	return_msg 128
- 随机字符串	wx_nonce_str 32 (return)
- 签名	wx_sign 32 (return) （关键） 
- 业务结果	result_code	 16
- 错误代码	err_code 32 
- 错误代码描述	err_code_des 128
- 交易类型	trade_type 16
- 预支付交易会话标识	prepay_id 64 (关键)
- 用户标识	openid 128
- 是否关注公众账号	is_subscribe 1 用户是否关注公众账号，Y-关注，N-未关注，仅在公众账号类型支付有效
- 付款银行	bank_type 16 
- 货币种类	fee_type 8
- 现金支付金额	cash_fee int 订单总金额-代金券或立减优惠金额=现金支付金额
- 代金券或立减优惠金额	coupon_fee int
- 代金券或立减优惠使用数量	coupon_count int
- 微信支付订单号	transaction_id 32 (关键)
- 支付完成时间	time_end 14
- 商户退款单号	out_refund_no	32
- 退款金额	refund_fee int
- 操作员	op_user_id 32 操作员帐号, 默认为商户号
- 微信退款单号	refund_id 32 
- 退款入账账户	refund_recv_accout 64 
- 退款状态	refund_status 16 

### 签证
### 活动
