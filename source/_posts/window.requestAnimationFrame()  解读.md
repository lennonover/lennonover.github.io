layout: window.requestanimationfram
title: window.requestanimationfram 解读
date: 2017-10-24 21:24:45
tags:
    - window.requestanimationfram
---

> [参考自张鑫旭老师](http://www.zhangxinxu.com/wordpress/2013/09/css3-animation-requestanimationframe-tween-%E5%8A%A8%E7%94%BB%E7%AE%97%E6%B3%95/)

window.requestAnimationFrame() 方法告诉浏览器您希望执行动画并请求浏览器调用指定的函数在下一次重绘之前更新动画。该方法使用一个回调函数作为参数，这个回调函数会在浏览器重绘之前调用。

## 语法

window.requestAnimationFrame(callback);

## 参数

`callback` 一个在每次需要重新绘制动画时调用的包含指定函数的参数。这个回调函数有一个传参，DOMHighResTimeStamp，指示从触发 requestAnimationFrame 回调到现在的时间（从 performance.now() 取得）。

## 返回值

一个 long 整数，请求 ID ，也是回调列表中唯一的标识。这是一个非零值，但你恐怕也不能从其值中作出什么臆想。你可以传此值到 window.cancelAnimationFrame() 以取消回调函数。

## 使用RequestAnimationFrame函数实现动画

我们经常使用 JavaScript 来实现动画效果，然而时机不当或长时间运行的 JavaScript 可能就是导致你性能下降的原因。显示器 16.7ms 刷新间隔之前发生了其他绘制请求(setTimeout)，导致所有第三帧丢失，继而导致动画断续显示这种做法的主要问题是回调将会在帧中的某个时间点运行，这可能会刚好在末尾，这就是过度绘制带来的问题。这也是为何 setTimeout 的定时器值推荐最小使用 16.7ms 的原因（16.7 = 1000 / 60, 即每秒 60 帧）。

![setTimeout](http://ob6nlbpso.bkt.clouddn.com/sp171024_220945.png)

有些第三方库仍在使用 setTimeout()&setInterval() 函数来实现动画效果，这会产生很多不必要的性能下降，例如老版本的 JQuery，如果你使用的是 JQuery3，那么不必为此担心，JQuery3 已经全面改写了动画模块，采用了requestAnimationFrame() 函数来实现动画效果。但如果你使用的是之前版本的 JQuery，那么就需要 jquery-requestAnimationFrame 来将 setTimeout() 替换为 requestAnimationFrame() 函数。本质上它与setTimeout()没有什么区别，都是在递归调用同一个回调函数来不断更新画面以达到动画的效果,优点：

- 就算很多个 requestAnimationFrame，浏览器只要通知一次就可以了。而 setTimeout 是多个独立绘制。
- 页面最小化了，或者被Tab切换关灯了，页面绘制全部停止，资源高效利用。

```
function updateScreen(time) {
    // 这是你的动画效果函数
}

// 将你的动画效果函数放入requestAnimationFrame()作为回调函数
requestAnimationFrame(updateScreen);
```

## 弊端 CSS3动画不能应用所有属性

类似滚动 scrollTop 是不支持的，这时需要 JS 来。