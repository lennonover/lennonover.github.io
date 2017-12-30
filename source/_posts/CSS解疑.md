---
title: CSS解疑
date: 2016-12-17 21:16:04
tags:
    - CSS
    - BFC
    - Box Model
---

# 1.BFC

> Block-level elements are those elements of the source document that are formatted visually as blocks (e.g., paragraphs). The following values of the 'display' property make an element block-level: 'block', 'list-item', and 'table'

浮动元素和绝对定位元素，非块级盒子的块级容器（例如 inline-blocks, table-cells, 和 table-captions），以及overflow值不为“visiable”的块级盒子，都会为他们的内容创建新的块级格式化上下文。

至少满足下列条件中的任何一个：

- float的值不为none

- position的值不为static或者relative

- display的值为 table-cell, table-caption, inline-block, flex, 或者 inline-flex中的其中一个

- overflow的值不为visible


## 规则：

在一个块级格式化上下文里，盒子从包含块的顶端开始垂直地一个接一个地排列，两个盒子之间的垂直的间隙是由他们的margin 值所决定的。毗邻块盒子的垂直外边距折叠只有他们是在同一BFC时才会发生。如果他们属于不同的BFC，他们之间的外边距将不会折叠。所以通过创建一个新的BFC我们可以防止外边距折叠。

在块级格式化上下文中，每一个盒子的左外边缘（margin-left）会触碰到容器的左边缘(border-left)（对于从右到左的格式来说，则触碰到右边缘），即使存在浮动也是如此，除非这个盒子创建一个新的块级格式化上下文。

- 内部的Box会在垂直方向，一个接一个地放置。

- Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠

- 每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。

- BFC的区域不会与float box重叠。

- BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。

- 计算BFC的高度时，浮动元素也参与计算 // 清除浮动

## 总结

BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。

因为BFC内部的元素和外部的元素绝对不会互相影响，因此， 当BFC外部存在浮动时，它不应该影响BFC内部Box的布局，BFC会通过变窄，而不与浮动有重叠。同样的，当BFC内部有浮动时，为了不影响外部元素的布局，BFC计算高度时会包括浮动的高度。避免margin重叠也是这样的一个道理。

# 2.box

## W3C的标准Box Model

    /*外盒尺寸计算（元素空间尺寸）*/
    Element空间高度 = content height + padding + border + margin
    Element 空间宽度 = content width + padding + border + margin
    /*内盒尺寸计算（元素大小）*/
    Element Height = content height + padding + border （Height为内容高度）
    Element Width = content width + padding + border （Width为内容宽度）


## IE传统下Box Model

    /*外盒尺寸计算（元素空间尺寸）*/
    Element空间高度 = content Height + margin (Height包含了元素内容宽度，边框宽度，内距宽度)
    Element空间宽度 = content Width + margin (Width包含了元素内容宽度、边框宽度、内距宽度)
    /*内盒尺寸计算（元素大小）*/
    Element Height = content Height(Height包含了元素内容宽度，边框宽度，内距宽度)
    Element Width = content Width(Width包含了元素内容宽度、边框宽度、内距宽度)

# box-size

    box-sizing ： content-box || border-box || inherit

## content-box

此值为其默认值，其让元素维持W3C的标准Box Model，也就是说元素的宽度/高度（width/height）等于元素边框宽度（border）加上元素内边距（padding）加上元素内容宽度/高度（content width/height）即：Element Width/Height = border+padding+content width/height。

## border-box

此值让元素维持IE传统的Box Model（IE6以下版本），也就是说元素的宽度/高度等于元素内容的宽度/高度。（从上面Box Model介绍可知，我们这里的content width/height包含了元素的border,padding,内容的width/height【此处的内容宽度/高度=width/height-border-padding】）。

# border

边框三角

![](http://ob6nlbpso.bkt.clouddn.com/17-1-3/35927458-file_1483451513816_13023.png)

# overflow

滚动条高度

![](http://ob6nlbpso.bkt.clouddn.com/17-1-3/58643265-file_1483456292463_15378.png)

解决出现滚动条后水平方向抖动问题

![](http://ob6nlbpso.bkt.clouddn.com/17-1-3/50929365-file_1483456274449_bb3e.png)

![overflow](http://ob6nlbpso.bkt.clouddn.com/17-1-6/66418111-file_1483699900475_6f2c.png)

## 去除图片和标签之间的空格

![](http://ob6nlbpso.bkt.clouddn.com/sp170107_195326.png)
