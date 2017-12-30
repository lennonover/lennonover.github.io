---
title: CSS之absolute
date: 2017-01-07 19:15:00
tags:
    - absolute
    - css
---

## 关于absolute

> 来自张鑫旭老师

- 脱离文档流
- 去浮动
- 超越overflow

绝对定位和浮动有相似性，同样具有浮动的包裹性和破坏性。一旦给元素加上absolute或float就相当于给元素加上了display:block。

设置为绝对定位的元素框从文档流完全删除并去浮动也不瘦overflow控制，并相对于其包含块定位，包含块可能是文档中的另一个元素或者是初始包含块。
绝对定位使元素的位置与文档流无关，因此不占据空间。

## 如何确定定位点

- 第一种情况

用户只给元素指定了absolute，未指定left/top/right/bottom。此时absolute元素的左上角定位点位置就是该元素`正常文档流`里的位置（也就是位置跟随）。

- 第二种情况

用户给absolute元素指定了left/right，top/bottom。位置是absolute元素没有position:static以外的父元素。
设置值得是相对没有position:static以外的父元素定位，没有设置值得还是在正常文档流中。

## 无依赖的绝对定位实战

优点对父级无依赖，自适应好

- 图片图表定位

利用跟随性实现

<iframe width="100%" height="300" src="//jsfiddle.net/lennonover/mzx6qc2r/embedded/html,css,result/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

- 边缘定位

``` html
<div class="course-fixed-x">
    &nbsp;<div class="course-fixed">
        <a href="http://www.imooc.com/activity/diaocha" class="goto_top_diaocha"></a>
        <a href="http://www.imooc.com/mobile/app" class="goto_top_app"></a>
        <a href="http://www.imooc.com/user/feedback" class="goto_top_feed"></a>
    </div>
</div>
```
```css
.course-fixed-x { height: 0;  text-align:right; overflow: hidden; }
.course-fixed { display: inline; position: fixed; margin-left: 20px; bottom: 100px; }
```

<iframe width="100%" height="300" src="//jsfiddle.net/lennonover/L6j0svgr/embedded/html,css,result/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>
