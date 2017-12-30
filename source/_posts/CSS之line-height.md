---
title: CSS之line-height
date: 2016-12-29 21:15:04
tags:
    - line-height
    - css
---
## 关于行高

> 来自张鑫旭老师

语法： line-height : normal |<实数> | <长度> | <百分比> | inherit     
说明：设置元素中行的高度。       
值： normal：默认行高，一般为1到1.2；
![normal](http://ww2.sinaimg.cn/large/c15a9505gw1fb80s9srh4j214c0ijmze.jpg)
![normal](http://ob6nlbpso.bkt.clouddn.com/sp161230_180949.png)
实数：实数值，缩放因子；
长度：合法的长度值，可为负数；
![length](http://ob6nlbpso.bkt.clouddn.com/sp161230_180903.png)
![](http://ob6nlbpso.bkt.clouddn.com/sp161229_222443.png)
百分比：百分比取值基于元素的字体尺寸。   
![%](http://ob6nlbpso.bkt.clouddn.com/sp161230_180923.png)   
初始值： normal     
继承性：继承      
适用于：所有元素        

## 行内框盒子模型

所有内敛元素的表现都和行内框盒子模型

- 内容区域（content area）

是一种围绕文字看不见的的盒子，大小和字体大小有关。

![内容区域](http://ww2.sinaimg.cn/large/c15a9505gw1fb7zns673ej210e0240t1.jpg)

- 内联盒子（inline boxes）

让内容排成一排显示，包含inline属性的都是，如果只是文字属于匿名内联盒子

![内联盒子](http://ww4.sinaimg.cn/large/c15a9505gw1fb7ztmap7ij210h02egm7.jpg)

- 行框盒子（line boxes）

每一行都是行框盒子，每个行框盒子都是由一个个内联盒子组成

![行框盒子](http://ww4.sinaimg.cn/large/c15a9505gw1fb7zwz3lixj210d02cgm1.jpg)

- 包含盒子（containing box）

由一行行行框盒子组成

![包含盒子](http://ww3.sinaimg.cn/large/c15a9505gw1fb7zze6n4fj20zj02ewf1.jpg)


包含盒子包含行框盒子，行框盒子包含内联盒子

## line-height的机制原理

有一个空的div，如果没有设置至少大于1像素高度height值时，该div的高度就是个0。如果该div里面打入了一个空格或是文字，则此div就会有一个高度。

这是个看上去很简单的问题，是理解line-height非常重要的一个问题。可能有人会跟认为是：文字撑开的！文字占据空间，自然将div撑开。我一开始也是这样理解的，但是事实上根本不是文字撑开了div的高度，而是line-height！

高度的表现不是行高，而是内容区域和行间距之和。

![line](http://ww4.sinaimg.cn/large/c15a9505gw1fb8069zl1zj20st06e74y.jpg)
![line](http://ww4.sinaimg.cn/large/c15a9505gw1fb807s4k58j212t0h3jug.jpg)
![line](http://ww2.sinaimg.cn/large/c15a9505gw1fb80eqerrqj214b0ljwi7.jpg)
![line](http://ww3.sinaimg.cn/large/c15a9505gw1fb80ezl5p8j214e0li0wc.jpg)
![line](http://ww2.sinaimg.cn/large/c15a9505gw1fb80fkfktlj214a0awgnk.jpg)

当行框盒子里有多个不同的行高的内联盒子此时行高？

## 使用经验

![body](http://ob6nlbpso.bkt.clouddn.com/sp161229_222811.png)
