---
title: JS继承
date: 2016-08-29 22:59:32
tags:
    - 原型
---
# 继承

一种减少重复性代码的一种设计模式，尽量弱化对象间耦合。在学习JS中继承过程中，遇到坑比较多，抽点时间记录下。由于javascript的语言特性，它的继承也被分为了3中实现方式。

##  对象创建

说道继承先来看看对象的创建

    //第一种，手动创建
    var a={'name':'hahaha'};   
    //第二种，构造函数
    function A(){
        this.name='hahaha';
    }
    var a=new A();
    //第三种，class (ES6标准写法)
    class A{
        constructor(){
            super();
            this.name='hahaha';
        }
    }
    var a=new A()
    //其实后面两种方法本质上是一种写法

这三种写法创建的对象的原型（父对象）都是Object,需要提到的是，ES6通过引入class ,extends等关键字，以一种语法糖的形式把构造函数包装成类的概念，更便于大家理解。是希望开发者不再花精力去关注原型以及原型链，也充分说明原型的设计意图和类是一样的。

## 构造函数继承
在子类型构造函数的内部调用超类构造函数，通过使用call()和apply()方法可以在新创建的对象上执行构造函数。

    function Father(name){
        this.name =name;
        this.age=5;
    }
    function Children(name){
        Father.call(this,name);
    }
    let jc = new Children("jicheng");
    let jc2 = new Children("jicheng2");
    jc2.age = 10;
    console.log(jc.name + jc.age);  //jicheng 5
    console.log(jc2.name + jc2.age);  //jicheng2 10

当我们new这个构造函数的时候，就会生成一个Children对象的实例。
但是通过上面的例子你会发现用构造函数生成实例对象，它有一个缺点，那就是无法共享属性和方法，没有涉及到原型。
因为这两个对象的age属性是独立的，修改其中一个，不会影响到另一个。
这样做的坏处就是会造成资源浪费，那么我们要如何来解决这件事呢，那就需要prototype出场了。

## 组合继承
就是融合了原型和构造器的一种继承方法。 这个应该算是一种比较稳妥的继承方式.即能排除引用类型造成的问题，又可以实现共享的效果。

    function Father(name){
        this.name =name;
    }
    Father.prototype.age=5;
    function Children(name){
        Father.call(this,name);
    }
    Children.prototype = new Father();
    Children.prototype.constructor = Children;
    let jc = new Children("jicheng");
    let jc2 = new Children("jicheng2");
    Father.prototype.age = 10;
    console.log(jc.name + jc.age);  //jicheng 10
    console.log(jc2.name + jc2.age);  //jicheng2 10

age属性放在prototype对象里，是两个实例对象共享的。只要修改了prototype对象，就会同时影响到两个实例对象。
这样会两次调用到父类型，对内存影响还是比较大的。但是es6 class  的出现，解决了这一问题。

## class
在使用class的使用，你完全可以不去写，constructor, prototype之类的东西了

    //原生继承方式   
    function Father(name){
        this.name =name;
    }
    Father.prototype.age=function(){
        return "4"
    };
    var father = new Father("张三")
    //使用class
    class Father(){
        constructor(name){
            this.name = name;
        }
        age(){
            return "4"
        }
    }
    var father = new Father("张三")


