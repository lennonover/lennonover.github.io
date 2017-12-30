---
title: ES5 bing 的实现
date: 2017-07-12 23:02:13
tags:
---
# bind函数的实现

> bind() 函数会创建一个新函数（称为绑定函数），新函数与被调函数（绑定函数的目标函数）具有相同的函数体（在 ECMAScript 5 规范中内置的call属性）。当目标函数被调用时 this 值绑定到 bind() 的第一个参数，该参数不能被重写。绑定函数被调用时，bind() 也接受预设的参数提供给原函数。一个绑定函数也能使用new操作符创建对象：这种行为就像把原函数当成构造器。提供的 this 值被忽略，同时调用时的参数被提供给模拟函数。

## 实现功能

- 返回新函数，调用新函数的返回值与调用旧函数的返回值相同
- 新函数的this指向bind的第一个参数
- 其余参数将在新函数后续被调用时位于其它实参前被传入
- 新函数也能使用new操作符创建对象，构造器为原函数

## 步骤

- 返回新函数，很简单，return一个包裹着旧函数调用的新函数

```javascript
function bind () {
  var t = this
  return function () {
    return t()
  }
}
```

- 新函数的this要指向bind的第一个参数，也简单，用js的call或apply函数

```javascript
function bind (that) {
  var t = this
  return function () {
    return tt.call(that)
  }
}
```

- 其余参数将在新函数后续被调用时在其它实参前被传入，类似实现curry化时通过闭包保存参数

```javascript
function bind (that) {
  var target = this
  var bindArgs = Array.prototype.slice.call(arguments, 1)
  return function () {
    var callArgs = Array.prototype.slice.call(arguments)
    return target.apply(that, bindArgs.concat(callArgs))
  }
}
```
这里的 Array.prototype.slice.apply(arguments) 指的是这个返回函数执行的时候传递的一系列参数，所以是从第一个参数开始 [0] ,之前的 args = slice.apply(arguments,1) 指的是 testBind 方法执行时候传递的参数，所以从第二个开始 [1]，两则有本质区别，不能搞混，只有两者合并了之后才是返回函数的完整参数

- 最后要达成的目标是在使用new操作符时，旧函数能作为构造器被调用。



函数被 new 实例化之后，需要继承原函数的原型链方法，且绑定过程中提供的 this 被忽略（继承原函数的 this 对象），但是参数还是会使用。这里就需要一个中转函数把原型链传递下去,我们需要看看 new 这个方法做了哪些操作 比如说 var a = new b()。
  - 创建一个空对象 a = {}，并且this变量引用指向到这个空对象a
  - 继承被实例化函数的原型 ：a.__proto__ = b.prototype
  - 被实例化方法b的this对象的属性和方法将被加入到这个新的 this 引用的对象中： b的属性和方法被加入的 a里面
  - 新创建的对象由 this 所引用 ：b.call(a)

```javascript
function bind (that) {
  var target = this
  var bindArgs = Array.prototype.slice.call(arguments, 1)
  function bound () {
    var callArgs = Array.prototype.slice.call(arguments)
    if (this instanceof fn) {
      return target.apply(this, callArgs.concat(callArgs))
    } else {
      return target.apply(that, callArgs.concat(callArgs))
    }
  }
  // 实现继承，让bound函数生成的实例通过原型链能追溯到target函数
  // 即 实例可以获取/调用target.prototype上的属性/方法
  var Empty = function () {}
  Empty.prototype = target.prototype
  bound.prototype = new Empty()  
  // 这里如果不加入Empty，直接bound.prototype = target.prototype的话
  // 改变bound.prototype则会影响到target.prototype，原型继承基本都会加入这么一个中间对象做屏障
  return bound
}
```

### 测试

```javascript
var test = function(a,b){
    console.log('作用域绑定 '+ this.value)
    console.log('testBind参数传递 '+ a.value2)
    console.log('调用参数传递 ' + b)
}
var obj = {
    value:'ok'
}
var fun_new = test.bind(obj,{value2:'also ok'})

fun_new ('hello bind')
```
