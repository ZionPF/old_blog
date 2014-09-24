---
layout: post
title: "R note : Data Cleaning"
description: ""
category: lessons
tags : [r]
---

#R learning note

在R语言学习和使用中,遇到的第一件事就是数据的获取和整理，这里把感觉比较实用和有趣的关于data cleaning的note记录在这里，不定时更新

----------------

##Data segmenting

对于一个较长的dataframe，可能需要对于某一个变量的不同区段进行分割。

例如把一个trace按照timestamp划分成不同时间区段的几个trace

首先用seq生成所需区段的间隔点，如

    range=seq(0,100,20)

之后使用cut能够将timestamp中的每个元素放置到range中去，如

    cut(flow_record$frame.time_relative,range)

这相当于对于timestamp进行了一次factor分类，于是可以依据此分类进行split：

    split(flow_record, cut(flow_record$frame.time_relative,range))
