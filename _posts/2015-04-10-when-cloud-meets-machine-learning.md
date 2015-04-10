---
layout: post
title: "When Cloud Meets Machine Learning"
description: ""
category: 
tags: [cloud, ml]
---

# ML one click away!

亚马逊刚刚发布了它的AML(Amazon Machine Learning), 蛮有意思的行动，大体上就是说他能够提供全套的数据处理、分析框架乃至一些经典算法，你只需要导入数据，
做一些简单的设定，点几下鼠标就好了！

[Here's the Link for AML blog page](https://aws.amazon.com/cn/blogs/aws/amazon-machine-learning-make-data-driven-decisions-at-scale/?from=timeline&isappinstalled=0)

这个想法，其实可以叫做 ML as a Service，之前和美国几个朋友也讨论过，甚至搭过一个简单的原型，就是你可以把csv file 拖拽进来，规定输入和分类向量，选定分类方法。。。

不同的是，我们的那个小Demo只是搭建在一个单一平台上，换句话说是一套系统处理所有数据请求，而Amazon必然不是这么干，这里面才涉及到问题的关键：

**能够根据数据来源、数据类型、数据量、计算复杂度进实时创建对应的处理单元！**

仔细想想，Amazon做这件事，可以算是水到渠成：存储有S3，网络有VPC。这两样保证了数据的管道和仓库的畅通，其实已经有很多人再AWS上面搭建自己的ML集群了，这些正式它能够进一步推出AML的基础。

现阶段，我不敢说多少人真的会用AML这样的套装，因为更多想学习ML的人，可能还是倾向于自己搭一套，对其中的各个组建和环节有些了解。但是我相信每个新兴技术在扩张期之后一定会走到服务水平。AML现在可能会被很多人看作是个玩具，几年前的OpenStack不也是么？
