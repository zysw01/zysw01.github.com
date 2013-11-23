---
layout: post
title: "R作组合图"
description: "R语言做组合图"
category: usedtools
tags: ["R"，"ggplot2","组合图"]
---
{% include JB/setup %}

###应用场景及需要工具

当需要在一个大图中画一个小图，以用于某种目的的比较。R语言可以使用grid包中viewport（）
函数。具体请看下面例子

```{r fig.width=7, fig.height=6}
library(ggplot2)#这里使用ggplot2作图
library(grid)#需要里面的viewport()函数
##pdf("tuzhongtu.pdf",width = 4, height = 4)#输出保存到pdf文件中,定义pdf的大小
subvp <- viewport(width = 0.4, height = 0.4, x = 0.75, y = 0.35)#定义图中图的大小及位置
b <- qplot(uempmed, unemploy, data = economics) + geom_smooth(se = F)#建立大图
print(b)#显示大图
c <- qplot(uempmed, unemploy, data = economics, geom = "path")#建立小图
print(c, vp = subvp)#将小图加入大图
##dev.off()#推出PDF画布
```


