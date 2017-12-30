---
title: ES6之Promise对象
date: 2016-10-12 20:58:40
tags:
    - Promise
---

# Promise
### 什么是Promise

ECMAScript 6中的Promise规范来源于Promises/A+社区。Promise是一个用于异步处理对象，其中包含了对异步进行各种操作的组件。Promise把JavaScript中的异步处理对象和处理规则进行了规范化，并按照统一的接口来编写，使用规定方法之外的写法会出现错误。

### 一个promise有下面三个不同状态：

*   pending待承诺         - 初始状态
*   fulfilled实现承诺  - 一个承诺成功实现状态
*   rejected拒绝承诺   - 一个承诺失败的状态

### promise必须实现 then 方法
*   then必须返回一个promise
*   可以调用多次
*   then方法接受两个参数

### Promise基本语法

#### 1、创建一个Promise对象

使用 new 来调用Promise的构造器来进行实例化

new Promise(fn) 返回一个promise对象
在fn 中指定异步等处理
处理结果正常的话，调用resolve(处理结果值) 
处理结果错误的话，调用reject(Error对象)

    var promise = new Promise(function(resolve, reject) {
    // 异步处理
    // 处理结束后、调用resolve 或 reject
    });

#### 2、了解Promise的状态

用 new Promise 实例化的promise对象有以下三个状态。

    "has-resolution" - Fulfilled

resolve(成功)时。此时会调用 onFulfilled

    "has-rejection" - Rejected

reject(失败)时。此时会调用 onRejected

    "unresolved" - Pending

既不是resolve也不是reject的状态。
也就是promise对象刚被创建后的初始化状态等

关于上面这三种状态的读法，其中 左侧为在 ES6 Promises 规范中定义的术语， 而右侧则是在 Promises/A+ 中描述状态的术语。

#### 3、Promise.then()

对通过 new 生成的promise对象为了设置其值在 resolve(成功) / reject(失败) 时调用的 回调函数 可以使用 promise.then() 实例方法。

    promise.then(onFulfilled, onRejected)
    resolve(成功)时
    onFulfilled 会被调用

    reject(失败)时
    onRejected 会被调用

onFulfilled、onRejected 两个都为可选参数。
promise.then 成功和失败时都可以使用。 如果只想对异常进行处理时可以采用 promise.then(undefined, onRejected) 这种方式，只指定reject时的回调函数即可。 不过这种情况下 promise.catch(onRejected) 应该是个更好的选择。


    var promise = new Promise(function(resolve, reject){
        resolve("传递给then的值");
    });
    // 写法一
    promise.then(function (value) {
        console.log(value);
    }, function (error) {
        console.error(error);
    });
    // 写法二（使用catch)
    promise.then(function (value) {
        console.log(value);
    }).catch(function (error) {
        console.error(error);
    });


#### 4、使用快捷方式

静态方法 Promise.resolve(value) 和 Promise.reject(error) 可以认为是 new Promise() 方法的快捷方式。

##### 4.1 Promise.resolve()

比如 Promise.resolve(1) 可以认为是以下代码的语法糖。

    new Promise(function(resolve){
        resolve(1);
    });

在这段代码中的 resolve(1)会让这个promise对象立即进入确定（即resolved）状态，并将 1 传递给后面then里所指定的 onFulfilled 函数。

方法 Promise.resolve(value) 的返回值也是一个promise对象，所以我们可以像下面那样接着对其返回值进行 .then 调用。

    Promise.resolve(1).then(function(value){
        console.log(value); // 1
    });

运用场景：Promise.resolve()在promise对象的初始化或者编写测试代码的时候都非常方便。

##### 4.2 Promise.reject()

比如 Promise.reject(new Error("出错了")) 就是下面代码的语法糖形式。

    new Promise(function(resolve,reject){
        reject(new Error("出错了"));
    });

方法 Promise.reject(error) 的返回值也是一个promise对象，所以可以将错误（Error）对象传递到catch里的函数中。

    Promise.reject(new Error("BOOM!")).catch(function(error){
        console.error(error);
    });

#### 5、Promise.all()

Promise.all 接收一个 promise对象的数组作为参数，当这个数组里的所有promise对象全部变为resolve或reject状态的时候，它才会去调用 .then 方法。

运用场景: 批量请求

    var p1 = Promise.resolve(1),
        p2 = Promise.resolve(2),
        p3 = Promise.resolve(3);
    Promise.all([p1, p2, p3]).then(function (results) {
        console.log(results);  // [1, 2, 3]
    });
    传递给 Promise.all 的promise并不是一个个的顺序执行的，而是同时开始、并行执行的。

    可以从以下例子的运行结果看出：

    // `delay`毫秒后执行resolve
    function timerPromisefy(delay) {
        return new Promise(function (resolve) {
            setTimeout(function () {
                resolve(delay);
            }, delay);
        });
    }
    var startDate = Date.now();
    // 所有promise变为resolve后程序退出
    Promise.all([
        timerPromisefy(1),
        timerPromisefy(32),
        timerPromisefy(64),
        timerPromisefy(128)
    ]).then(function (values) {
        console.log(Date.now() - startDate + 'ms');
        // 約128ms
        console.log(values);    // [1,32,64,128]
    });


#### 6、Promise.race()

Promise.race 只要有一个promise对象进入 FulFilled 或者 Rejected 状态的话，就会继续进行后面的处理。

    var p1 = Promise.resolve(1),
        p2 = Promise.resolve(2),
        p3 = Promise.resolve(3);
    Promise.race([p1, p2, p3]).then(function (value) {
        console.log(value);  // 1
    });
    进一步分析, 当第一个promise对象变为确定（FulFilled）状态后，它之后的promise对象是否还在继续运行。
    
    // `delay`毫秒后执行resolve
    function timerPromisefy(delay) {
        return new Promise(function (resolve) {
            setTimeout(function () {
                console.log(delay)
                resolve(delay);
            }, delay);
        });
    }
    // 任何一个promise变为resolve或reject 的话程序就停止运行
    Promise.race([
        timerPromisefy(1),
        timerPromisefy(10),
        timerPromisefy(20),
    ]).then(function (value) {
        console.log(value);    // => 1
    });
    //整体运行结果
    1
    1
    10
    20

执行上面代码，我们会看到setTimeout 方法都会执行完毕， console.log 也会分别输出它们的信息。也就是说， Promise.race 在第一个promise对象变为Fulfilled之后，并不会取消其他promise对象的执行。

#### Promise实例练习

考虑本文一开始的场景，进行小进阶练习，实现每隔一秒依次输出1-10。

    //利用for循环 避免一长串的.then写法
    function log(i) {
        return new Promise(function(resolve) {
            setTimeout(function() {
                console.log(i)
                resolve();
            }, 1000)
        })
    }
    function printN(n) {
        var p = log(1);
        for (var i = 2; i <= n; i++) {
            var a = function(x) { // 注意回调函数闭包的问题
                return function() {
                    return log(x);
                }
            }
            p = p.then(a(i));
        }
    }
    printN(10);

