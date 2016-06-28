+++
categories = ["约定"]
date = "2016-06-28T19:00:23+08:00"
draft = false
tags = ["json约定", "设计"]
title = "关于签证申请填写信息的设计及约定"

+++
### 原则

不破坏原有的数据库和订单设计，将新增的数据采用以下约定的json格式存储到相关备注字段。

### 表字段的对应关系

- **出行日期、发票抬头、电子邮件** json格式 存入 **order_v2.remark**

~~~
{"out_date" : "2016-06-01",
 "invoice" : "xxx公司",
 "email" : "xxx@qq.com"}
~~~
- **办理人数** 存入 **detail.quantity**
- **境外旅行意外险** 存入 **order_v2.delivery_fee**，这里记录保险单价和人数的乘积
保险的单价存储为全局变量
- **联系人、联系电话、资料返回地址** 存入 **user_delivery_v2** 的对应字段
- **申请人、客户类型** json数组格式 存入 **order_detail.remark**

~~~
{"applys" : 
  [
    {
      "name": "小燕",
      "type": "在职"
    },
    {
      "name": "小小燕",
      "type": "在校学生"
    }
  ]
}
~~~

