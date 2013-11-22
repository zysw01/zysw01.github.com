---
layout: post
title: "假设检验相关分析"
description: "包括T检验、F检验、非参数检验、卡方检验、控制变量的方法及ABtest方法"
category: studynotes
tags: ["假设检验","运营分析","R实现"]
---
{% include JB/setup %}
####假设检验的应用背景

1、**精确区分**并**在多大置信度内肯定**运营效果的差别是随机因素引起的，还是运营因素引起的。

2、从样本的结论推导总体的结论，必须进行假设检验来判断样本的差异能否代表总体的差异，同时还要确定样本的差异在多大置信度内可以代表总体的差异。

####假设检验基本思想

只有两种结果的判断，选择要么A，要么B；要么显著，要么不显著；要么达标，要么不达标；这些都是是非判断。这两种选择就对应两个假设：原假设H0（Null Hypothesis），备选假设H1（Alternative）。

关于正态分析的检验R一般使用`shapiro.test()`来提供W统计量和相应P值，当P值小于某个显著性水平a（比如0.05），则拒绝原假设：样本来自正态分析的总体，选择备选假设：样本不是来自正态分布的总体。