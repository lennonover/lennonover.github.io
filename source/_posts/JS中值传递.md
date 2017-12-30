---
title: JS中值传递
date: 2017-02-21 21:29:00
tags:
  - javascript
  - 引用传递
  - 值传递
---

## 按值传递 VS 按引用传递

 - 按值传递(call by value)

 函数的形参是被调用时所传实参的副本。修改形参的值并不会影响实参。

- 按引用传递(call by reference)

  函数的形参接收实参的隐式引用，而不再是副本。这意味着函数形参的值如果被修改，实参也会被修改。同时两者指向相同的值。

## 探究JS值的传递方式

JS的基本类型，是按值传递的。

```javascript
var c = 3;
function foo(x) {
    x = 9;
}
foo(c);
console.log(c); // 仍为3, 未受x = 9赋值所影响
```

对象

```javascript
var obj = {x : 1};
function foo(o) {
    o.x = 3;
}
foo(obj);
console.log(obj.x); // 3, 被修改了!
```
说明o和obj是同一个对象，o不是obj的副本。所以不是按值传递。 但这样是否说明JS的对象是按引用传递的呢？我们再看

下面的例子：

```javascript
var obj = {x : 1};
function foo(o) {
    o = 100;
}
foo(obj);
console.log(obj.x); // 仍然是1, obj并未被修改为100.
```
如果是按引用传递，修改形参o的值，应该影响到实参才对。但这里修改o的值并未影响obj。 因此JS中的对象并不是按引用传递。那么究竟对象的值在JS中如何传递的呢？

## 按对象传递

JS中的基本类型按值传递，对象类型按按对象传递.
数组、对象是把变量地址复制进去的。
```javascript
var a = []
var b = {};
var c = {};
function foo(a, b, c)
{
    a = [1];
    b = [2];
    c = {d:3}
}

foo(a, b, c);
alert (a); // 空白
alert (b); // [object Object]
alert (c.d); // undefined
```
上面我们让 v1、v2、v3 作为参数进入函数后，就有了地址副本，这些地址副本的指向和外面的 v1、v2、v3 的地址指向是相同的。但我们`为 v1、v2、v3 赋了值`，也就是说我们把地址副本的指向改变了，指向了新的数组和对象。这样内部的 v1、v2、v3 和外部的 v1、v2、v3 就完全断了。
如果我们不赋新值，而是直接操作它，那么，它操作到的，仍然是和外面的 v1、v2、v3 指向的同一块数组或对象。

所以传值的比较比较的是数值 而传址的比较比较的是引用，引用不同即使数值相同也不等

```javascript
1 == 1;    //true
1 === 1;   //true
[0] == [0]; //false
[0][0] == [0][0];    //true
[0][0] === [0][0];   //true
[0].toString() == [0].toString();   //true    
```
