---
title: JS类型检测
date: 2017-02-20 22:01:55
tags:
  - javascript
  - 数据类型
---
# 数据类型

JavaScript中常见的几种数据类型：

基本类型：string,number,boolean,undefined,null

引用类型：Object,Function,Function,Array,Date等

## typeof

typeof 返回一个表示数据类型的字符串，返回结果包括：number、boolean、string、object、undefined、function等6种数据类型。
typeof 可以对JS基础数据类型做出准确的判断。

|Value               |Class      |Type|
| ------| ------ | ------ |
|"foo"               |String     |string|
|new String("foo")   |String     |object|
|1.2                 |Number     |number|
|new Number(1.2)     |Number     |object|
|true                |Boolean    |boolean|
|new Boolean(true)   |Boolean    |object|
|new Date()          |Date       |object|
|new Error()         |Error      |object|
|[1,2,3]             |Array      |object|
|new Array(1, 2, 3)  |Array      |object|
|new Function("")    |Function   |function|
|/abc/g              |RegExp     |object| (function in Nitro/V8)|
|new RegExp("meow")  |RegExp     |object| (function in Nitro/V8)|
|{}                  |Object     |object|
|new Object()        |Object     |object|

Type 一列表示 typeof 操作符的运算结果。可以看到，这个值在大多数情况下都返回 "object"。

Class 一列表示对象的内部属性 [[Class]] 的值。

> typeof foo !== 'undefined'

上面代码会检测 foo 是否已经定义；如果没有定义而直接使用会导致 ReferenceError 的异常。 这是 typeof 唯一有用的地方。除非为了检测一个变量是否已经定义，我们应尽量避免使用 typeof 操作符。

## Object.prototype.toString
公认靠谱的方法。
JavaScript 标准文档只给出了一种获取 上表  [[Class]] 值的方法，那就是使用 Object.prototype.toString，
该方法默认返回其调用者的具体类型。

```
function is(type, obj) {
    var clas = Object.prototype.toString.call(obj).slice(8, -1);
    return obj !== undefined && obj !== null && clas === type;
}

is('String', 'test'); // true
is('String', new String('test')); // true
```
更严格的讲，是 toString运行时this指向的对象类型, 返回的类型格式为[object,xxx],xxx是具体的数据类型。
```
Object.prototype.toString.call('') ;   // [object String]
Object.prototype.toString.call(1) ;    // [object Number]
Object.prototype.toString.call(true) ; // [object Boolean]
Object.prototype.toString.call(undefined) ; // [object Undefined]
Object.prototype.toString.call(null) ; // [object Null]
Object.prototype.toString.call(new Function()) ; // [object Function]
Object.prototype.toString.call(new Date()) ; // [object Date]
Object.prototype.toString.call([]) ; // [object Array]
Object.prototype.toString.call(new RegExp()) ; // [object RegExp]
Object.prototype.toString.call(new Error()) ; // [object Error]
Object.prototype.toString.call(document) ; // [object HTMLDocument]
Object.prototype.toString.call(window) ; //[object global] window是全局对象global的引用
```
必须通过Object.prototype.toString.call来获取，而不能直接 new Date().toString(),因为大部分的对象都实现了自身的toString方法，这样就可能会导致Object的toString被终止查找，因此要用call来强制执行Object的toString方法。

## instanceof
instanceof 操作符用来比较两个操作数的构造函数例如来判断 A 是否为 B 的实例。只有在比较自定义的对象时才有意义。
>[] instanceof Array     // true
 [] instanceof Object;   // true

instanceof检测的是原型。
