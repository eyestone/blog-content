+++
categories = ["design"]
date = "2016-08-10T13:22:09+08:00"
draft = false 
tags = ["设计","约定"]
title = "订单状态定义"

+++
#### 订单状态StatusCode字段定义

注：已约定的不会变化，但会根据业务发展有增补

        /*****************订单状态***********************/
        public static final Integer DELETE          = 0;//删除,仅逻辑删除，app不显示 
        public static final Integer NOT_PAY         = 1;//等待买家付款 
        public static final Integer BUYER_PAYED     = 2;//商户端:买家已付款 / app端:等待卖家发货

        public static final Integer SELLER_SEND     = 3;//app:卖家已发货
        public static final Integer BUYER_RECEIVED  = 4;//商户端:买家已收货
        public static final Integer EVALUATED       = 5;//已评价
        public static final Integer RE_EVALUATED    = 6;//追加评价
        public static final Integer CANCEL          = 7;// 取消
        public static final Integer SUBMIT_BUSI     = 10;//订单提交给供应商

        /*****************交易状态***********************/
        public static final Integer TRADE_SUCCESS   = 8;//交易成功
        public static final Integer TRADE_CLOSE     = 9;//交易关闭

        /*****************退款状态***********************/
        public static final Integer REFUND_APPLY    = 11;//申请退款
        public static final Integer REFUND_AGREE    = 12;//同意退款
        public static final Integer REFUND_SUCCESS  = 13;//退款成功
        public static final Integer REFUND_CLOSE    = 14;//退款关闭

        /*****************换货状态***********************/
        public static final Integer EXCHANGE_APPLY  = 21;//申请换货
        public static final Integer EXCHANGE_AGREE  = 22;//同意换货
        public static final Integer EXCHANGE_SUCCESS= 23;//换货成功
        public static final Integer EXCHANGE_CLOSE  = 24;//换货关闭

        /*****************退货退款状态**********************/
        public static final Integer RETURN_APPLY    = 31;//申请退货
        public static final Integer RETURN_AGREE    = 32;//同意退货
        public static final Integer RETURN_SUCCESS  = 33;//退货成功
        public static final Integer RETURN_CLOSE    = 34;//退货关闭

