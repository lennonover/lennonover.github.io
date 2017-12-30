---
title: CSS之vertical-align
date: 2016-12-30 20:05:52
tags:
    - css
    - vertical-align
---
## 关于vertical-align

> 来自张鑫旭老师

vertical-align 属性设置元素的垂直对齐方式。       
vertical-align属性的值有baseline、sub、super、top、text-top、middle、bottom、text-bottom等，其中初始值为baseline。

支持四大类

![](http://ob6nlbpso.bkt.clouddn.com/sp161230_202002.png)

> 注：vertical-align的数值百分比是根据line-height计算的

- baseline(基线)——将子元素的基线与父元素的基线相对齐。对于没有基线的元素，如图像或对象，则使它的底部与父元素的基线对齐。ppps：内联元素是基线对齐，文本的基线一般是`X`的下边缘。

- top(顶部)——使元素的顶部与行中最高事物的顶部相对齐。

- middle(中间)——使元素垂直居中。

- bottom(底部)——使元素的底部与行中最低事物的底部相对齐。

- text-top(文本顶部)——使元素的顶部与其父元素最高字母的顶部相对齐。

- text-bottom(文本底部)——使元素的底部与其父元素字体的底部相对齐。

- sub(下面)——把元素置于下方(下标)，确切地说是使元素的基线对齐它的父元素首选的下标位置。

- super(上面)——把元素置于上方(上标)，确切地说是使元素的基线对齐它的父元素首选的上标位置。

![](http://ob6nlbpso.bkt.clouddn.com/sp170101_165106.png)

## vertical-align起作用的前提条件

符合以下条件

![](http://ob6nlbpso.bkt.clouddn.com/sp161230_202811.png)
![](http://ob6nlbpso.bkt.clouddn.com/sp161230_202901.png)

> table-call只作用于自身

- 直接display设置
    display

- css声明更改元素显示水平
    float、position

## 应用

<iframe width="100%" height="600" src="//jsfiddle.net/lennonover/j5unucec/embedded/html,css,result/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

## 兼容性
