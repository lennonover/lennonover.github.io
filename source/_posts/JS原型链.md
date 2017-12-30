---
title: JS原型链
date: 2017-01-21 20:23:20
tags:
    - 原型链
---
# 名词解释

## 函数对象

由Function创造出来的函数 eg：

```javascript
function a(){};
var b=function(){};
```

系统内置的函数对象

```javascript
Function,Object,Array,String,Number
```

> 只有函数对象才有 prototype属性

> 引用类型值：指的是那些保存在堆内存中的对象，意思是，变量中保存的实际上只是一个指针，这个指针执行内存中的另一个位置，由该位置保存对象,那么数组,普通对象,函数对象都算是引用数据类型,引用数据类型范围包含函数对象的范围

## 普通对象

函数对象之外的对象都是普通对象

>基本类型值：指的是保存在栈内存中的简单数据段；除开函数对象之外的对象都是普通对象,那么普通对象范围是包含基本数据类型的

## 原型对象

prototype属性

```javascript
function a(){};
```

对象 `a` 是由`Function`创造出来,是函数对象,`a` 就有了`prototype`属性.那么这个原型对象是怎么创造出来的呢?
来看下面这个例子:

```javascript
var temp = new a();
a.prototype = temp;
```
a的prototype属性就是这样创造出来的


## 指针__proto__

所有的对象obj都具有proto属性(null和undefined除外),而且指向创造obj对象的函数对象的prototype属性 eg:

```javascript
function a(){};
console.log(a.prototype.__proto__===Object.prototype); //true
console.log(a.__proto__===Function.prototype); //true 指向创造obj对象的函数对象的prototype属性
```
eg :

```javascript
// 在JavaScript的世界中，所有的函数都能作为构造函数，构造出一个对象
// 下面我给自己构造一个女神做对象
function NvShen () {
 this.name = "Alice";
}
// 现在我设置NvShen这个函数的prototype属性
// 一般来说直接用匿名的对象就行，我这里是为了方便理解，
// 先定义一个hand对象再把hand赋值给NvShen的prototype
var hand = {
 whichOne: "right hand",
 someFunction: function(){
   console.log("not safe for work.");
 }
};
NvShen.prototype = hand;

// 这个时候，我们可以用NvShen作为构造函数，构造出myObject对象
var myObject = new NvShen();
console.log(myObject.__proto__ === NvShen.prototype) //true
```

我们构建了一个对象myObject，而myObject的原型是hand对象，而刚好myObject的构造函数NvShen()的prototype属性也指向hand对象。现在我们知道，prototype与__proto__的关系就是：你的__proto__来自你构造函数的prototype
hand.__proto__ 指向的是Object.prototype

> 思考：var obj={}; obj.prototype.__proto__指向谁__?

```javascript
这里分步思考:
1, obj是是一个普通对象
2, 什么类型的对象是由prototype属性的?当然是函数对象
3, 所以obj是没有prototype属性的
4, 所以obj.prototype===undefined
5, 所以此题的最终问题是:undefined.proto指向什么
6, 所有的对象obj都具有proto属性(null和undefined除外)!所以答案是 js报错
```   



## 构造器constructor

constructor 属性返回对创建此对象的函数对象的引用。

```javascript
function a(){};
console.log(a.constructor===Function); //true
console.log(a.prototype.constructor===a); //true
```
函数a是由Function创造出来,那么它的constructor指向的Function,
a.prototype是由new a()方式创造出来,那么a.prototype.constructor理应指向a

>思考:a.prototype.__proto__.constructor指向谁__?

```javascript
这里继续分解题目:
1, a.prototype指向a的一个实例,我们已经多次强调了,而且属于普通对象
2, __proto__定义为:指向创造obj对象的函数对象的prototype属性,所以看下谁创造了a.prototype,因为a.prototype是普通对象,类型为object,那么是Object创造了它,
3, 那么显而易见a.prototype.__proto__指向了Object.prototype
4, 那么题目简化为Object.prototype.constructor指向谁
5, 继续分解题目,Object.prototype为基本对象,那么就是Object创造了它,那么它的constructor就指向了Object
```

```javascript
Object.prototype.constructor===Object //true
Object.prototype.__proto__===null //true
```
## 原型链


JS在创建对象（不论是普通对象还是函数对象）的时候，都有一个__proto__的内置属性，用于指向创建它的函数对象的原型对象prototype。

```javascript
console.log(a.__proto__ === person.prototype) //true
```
同样，person.prototype对象也有__proto__属性，它指向创建它的函数对象（Object）的prototype

```javascript
console.log(person.prototype.__proto__ === Object.prototype) //true
```
继续，Object.prototype对象也有__proto__属性，但它比较特殊，为null

```javascript
console.log(Object.prototype.__proto__) //null
```
我们把这个有__proto__串起来的直到Object.prototype.__proto__为null的链叫做原型链。
