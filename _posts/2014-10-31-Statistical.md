---
layout: post
title: "R note : Statistical Analysis"
description: ""
category: lessons
tags : [r]
---

#R learning note

对于统计，R天生好用，这里给一些目前用到的统计方面的函数，持续更新：
----------------

##Max, Min, Mean, Median

    max(), min(), mean(), median(), var(), sd() ...

不用多说，显而易见。值得注意的是关于NA值的处理。

##Percentile

    duration = faithful$eruptions
    quantile(duration, c(.32, .57, .98))
    32%    57%    98% 
    2.3952 4.1330 4.9330

即这组数据中，32%的数据是小于等于2.3952的，如此类推。这个值在统计中很有用，直接表现就是CDF。

##Hist

    hist(faithful$eruptions)

    $breaks
    [1] 1.5 2.0 2.5 3.0 3.5 4.0 4.5 5.0 5.5

    $counts
    [1] 55 37  5  9 34 75 54  3

    $density
    [1] 0.40441176 0.27205882 0.03676471 0.06617647 0.25000000 0.55147059 0.39705882 0.02205882

    $mids
    [1] 1.75 2.25 2.75 3.25 3.75 4.25 4.75 5.25

    $xname
    [1] "duration"

    $equidist
    [1] TRUE

    attr(,"class")
    [1] "histogram"

Hist 则是典型的计数分布图，即每个区间有多少次样本出现。直接表现就是PDF。
