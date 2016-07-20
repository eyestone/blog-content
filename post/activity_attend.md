+++
categories = ["约定"]
date = "2016-06-27T14:44:28+08:00"
draft = false 
tags = ["json", "活动"]
title = "关于活动报名信息的json格式约定"
author = "freeopen"
+++

*author : freeopen*

### json格式说明

这里的json格式将对应app上不同的可视控件内容，详细如下：

- topic 标题
- type 控件类型 
    - one       单选
    - more      多选
    - number    数字输入框
    - text      文本输入框
    - date      日期输入框

- tagId 自定义的id编号，相当于数组索引，方便代码快速检索
- tagName 当控件类型为 单选 和 多选 时有效，即选项信息
- tagStatus  当类型为单选和多选时，记录用户的选中情况, 其中为1时，表示缺省值，另外用户选中时为1，取消时为0。当为输入框时，记录输入内容。

~~~
{
    "list":[
        {
            "topic":"性别",
            "type":"one",
            "tag":[
                {
                    "tagId":11,
                    "tagName":"男",
                    "tagStatus":""
                },
                {
                    "tagId":12,
                    "tagName":"女",
                    "tagStatus":""
                }
            ]
        },
        {
            "topic":"宝宝年龄",
            "type":"one",
            "tag":[
                {
                    "tagId":21,
                    "tagName":"0-2",
                    "tagStatus":""
                },
                {
                    "tagId":22,
                    "tagName":"3-4",
                    "tagStatus":""
                },
                {
                    "tagId":23,
                    "tagName":"5-6",
                    "tagStatus":""
                }
            ]
        },
        {
            "topic":"陪同家长",
            "type":"more",
            "tag":[
                {
                    "tagId":31,
                    "tagName":"爸爸",
                    "tagStatus":""
                },
                {
                    "tagId":32,
                    "tagName":"妈妈",
                    "tagStatus":""
                },
                {
                    "tagId":33,
                    "tagName":"爷爷",
                    "tagStatus":""
                },
                {
                    "tagId":34,
                    "tagName":"奶奶",
                    "tagStatus":""
                }
            ]
        },
        {
            "topic":"手机号码",
            "type":"number",
            "tag":[]
        },
        {
            "topic":"可参加日期",
            "type":"date",
            "tag":[]
        }
    ]
}
~~~

