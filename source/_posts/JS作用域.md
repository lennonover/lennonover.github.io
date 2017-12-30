---
title: JS作用域
date: 2016-07-08 23:53:28
tags:
  - JavaScript
---

## JavaScript的作用域链
JavaScript需要查询一个变量x时，首先会查找作用域链的第一个对象，如果以第一个对象没有定义x变量，JavaScript会继续查找有没有定义x变量，如果第二个对象没有定义则会继续查找，以此类推。下面的代码涉及到了三个作用域链对象，依次是：inner、rainman、window。
```plain
    var rain = 1;
    function rainman(){
        var man = 2;
        function inner(){
            var innerVar = 4;
            //var rain = 5;//函数体内部，局部变量的优先级比同名的全局变量高。
            alert(rain);
        }
        inner();    //调用inner函数 1
    }
    rainman();    //调用rainman函数
```

## JavaScript没有块级作用域
比如if条件语句，就不算一个独立的作用域.
为了解决块级作用域，
ES6引入了新的关键字let，用let替代var可以申明一个块级作用域的变量.
ES6标准引入了新的关键字const来定义常量，const与let都具有块级作用域：
```plain
	function func() {
	  if (true) {
	    var tmp = 123;
	  }
	  console.log(tmp); // 123 var关键字声明的变量有一个变量提升的过程
	}

	function func() {
	  if (true) {
	    let tmp = 123;
	  }
	  console.log(tmp); // ReferenceError: tmp is not defined
	}

	const PI = 3.14;
	PI = 3; // 某些浏览器不报错，但是无效果！
	PI; // 3.14
```

## 函数中声明的变量在整个函数中都有定义
由于在函数rain内局部变量x在整个函数体内都有定义（ var x= 'rain-man'，进行了声明），所以在整个rain函数体内隐藏了同名的全局变量x。这里之所以会弹出'undefined'是因为，第一个执行alert(x)时，局部变量x仍未被初始化。
```plain
	var x = 1;
    function rain(){
        alert( x );        //弹出 'undefined'，而不是1
        var x = 'rain-man';
        alert( x );        //弹出 'rain-man'
    }
    rain();
    //等效于下面
    var x = 1;
    function rain(){
	    var x;
	    alert( x );
	    x = 'rain-man';
	    alert( x );
	}
    rain();
```

## 未使用var关键字定义的变量都是全局变量、全局变量都是window对象的属性
