---
title: CSS 动画
date: 2017-10-15 22:54:23
tags:
---

## 转换（transition）

页面元素从旧的属性值立即变成新的属性值的效果。Transition (转变)能让页面元素不是立即的、而是慢慢的从一种状态变成另外一种状态，从而表现出一种动画过程。

    ```CSS
    transition-property – 什么属性将用动画表现，例如, opacity。       

    transition-duration – 转变过程持续时间。     

    transition-timing-function – 转变时使用的调速函数(比如, linear、 ease-in 或自定义的 cubic bezier 函数)。     
        linear – 线性函数，返回值一个输入值一样的结果。        
        ease – 减缓函数, 是缺省值, 等同于 cubic-bezier(0.25, 0.1, 0.25, 1.0).      
        ease-in – 等同于 cubic-bezier(0.42, 0, 1.0, 1.0).      
        ease-out – 等同于 cubic-bezier(0, 0, 0.58, 1.0).       
        ease-in-out – 等同于 cubic-bezier(0.42, 0, 0.58, 1.0)      
    ```

<iframe width="100%" height="300" src="//jsfiddle.net/lennonover/hf83eckt/embedded/html,css,result/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

## 变形（transform）

    - 旋转 rotate（中心为原点）
        transform: rotate(10deg)，其中“10deg”表示“10度”。

    - 扭曲、倾斜skew（skew(x,y), skewX(x), skewY(y)）
        transform: skew(20deg);

    - 缩放scale（scale(x,y), scaleX(x), scaleY(y)）
        transform: scale(1.5);

    - 移动translate（translateX,translateY）
        transform: translate(120px,0)，如下表示向右位移120像素，如果向上位移，把后面的“0”改个值就行，向左向下位移则为负“-”。

    - 矩阵变形matrix。


## 关键帧动画

语法：

animation: name duration timing-function delay iteration-count direction;   

    - animation-name	规定需要绑定到选择器的 keyframe 名称。
    - animation-duration	规定完成动画所花费的时间，以秒或毫秒计。
    - animation-timing-function	规定动画的速度曲线。
    - animation-delay	规定在动画开始之前的延迟。
    - animation-iteration-count	规定动画应该播放的次数。
    - animation-direction	规定是否应该轮流反向播放动画。
    
<iframe width="100%" height="300" src="//jsfiddle.net/lennonover/ev21dw39/embedded/html,css,result/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>


## transition 和 animation 主要区别

transition 也可以看做animation 的缩略版，transition 是着重于属性的变化，而animation 重点是在时间轴和关键帧,是在于创建帧，让不同帧在不同的时间节点发生不同变化，基于animation和@keyframe 的动画一方面也是为了实现表现与行为的分离，另一方面也使得前端设计师可以专注得进行动画开发而不是冗余的代码中。