---
title: Js 异常处理
date: 2017-09-18 23:24:29
tags:
    - javascript
---

# Js 异常

JavaScript 一般有三种错误，语法错误、运行期错误、逻辑错误。

- 语法错误

语法错误同样也被称为解析错误，对于传统的编程语言，该错误出现先编译的时候，对于 JavaScript 该错误出现在解释时期，最常见比如打了中文分号，语法错误不会通过解析器。

- 运行时错误

运行期错误也被称为异常，通常在编译或者解释之后运行时会出现。异常出现时会影响进程创建时的正常执行，但是允许对于其他的 JavaScript 进程则可以继续正常执行。

#### JavaScript 提供了特殊的语句来捕捉异常。

- try...catch...finally 语句

JavaScript 的最新版本中添加了异常处理的功能。它实现了 try...catch...finally 结构和 throw 操作用来处理异常。 你可以捕获程序员和运行期产生的异常，但是对于用户不能捕获 JavaScript 的语法错误。

```javascript
function A() {
    throw new Error("A报错");
}
function B() {
    try {
        A(); 
    } catch(error) {
        console.log(error);
    }
}
B()
```

- throw 语句

用 throw 语句产生一个内置的异常或者你自己定制的异常。之后这些异常可以被捕获，而且捕获后你可以采取合适的操作

```javascript
function A(a){
    if ( a === 0 ){
    }else{
        var err =  new Error("a 不等于0");
        err.statusCode = 000;  
        throw err;
    }
}
A(1);
```

- onerror() 方法

onerror 事件句柄是 JavaScript 中添加的第一个为了方便错误处理的特性。无论任何时候在网页中产生了异常，窗口对象就会触发 error 事件   

- 错误消息。 浏览器显示给定错误的相关信息。
- URL。 错误出现的文件。
- 行数。 在指定的 URL 中造成错误的行数。
```javascript
window.onerror = function (msg, url, line) {
    alert("Message : " + msg );
    alert("url : " + url );
    alert("Line number : " + line );
}
```

#### 对于异常处理

- 预期异常：参数不合法，前提条件不符合，通常直接 throw
- 非预期异常: js 运行时异常，来着依赖库异常
- 可以直接在异常上面提供一下附加属性来提供上下文

#### 异步回调异常处理

try catch 无法捕获异步队列中抛出的异常，因为执行栈执行完，异步队列才开始执行，所以执行栈无法捕获异步
函数抛出的异常     
对于异步函数，我们对异常的处理原则为, 异步函数里面使用自己的try catch 
自处理异常

```javascript
function asyncCallbackError(callback) {
    setTimeout(function(){
        try {
            if (发送了异常) {
                throw new Error("异步出现了异常");
            }
            else  没有异常 {
                callback(null, value);  无异常的话 第一个参数设置为null 第二个自己的值
            }
        } catch(error) {
            callback(error);
        }
    }, 0);
}
function callAsync() {
    //callback 判断获取异步异常值
    asyncCallbackError(function(error, value){
        error ? console.log(error) : "执行方法";
    });
}
callAsync();
```
