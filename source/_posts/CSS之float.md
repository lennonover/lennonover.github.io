---
title: CSS之float
date: 2017-01-01 18:25:17
tags:
    - float
    - css
---

## 浮动的原始作用

> 来自张鑫旭老师

实现文字环绕图片

特性  

![](http://ob6nlbpso.bkt.clouddn.com/17-1-1/18915576-file_1483272315295_15747.png)

本质定义为包裹与破坏。

- 包裹性

例如按钮宽度自适应，按钮要自动包裹在文字的外面。我们用什么方法实现呢？一就是display:inline-block；二就是float。

- 破坏性

文字之所以会环绕含有float属性的图片时因为浮动破坏了正常的line boxes。
脱离文档流，父元素感受不到子元素，也就没了高度。

## 清除浮动

- clear

![](http://ob6nlbpso.bkt.clouddn.com/17-1-1/84516968-file_1483271906445_14f39.png)
![](http://ob6nlbpso.bkt.clouddn.com/17-1-1/28721104-file_1483272033640_f4b2.png)

- BFC

![](http://ob6nlbpso.bkt.clouddn.com/17-1-1/11863236-file_1483271975963_16541.png)
![](http://ob6nlbpso.bkt.clouddn.com/17-1-1/63092528-file_1483272033530_381d.png)

兼容性

![](http://ob6nlbpso.bkt.clouddn.com/17-1-1/82732728-file_1483272086314_3785.png)

## 应用

![](http://ob6nlbpso.bkt.clouddn.com/17-1-1/34179867-file_1483272176105_b73a.png)
