title: 优雅的for循环
date: 2017-03-08 22:30:40
tags:
---
## 来源

最近做了一个维度筛选的查询项目，复杂的交互写的一脸懵逼，最后总算功能实现，回过头代码重构，满屏的for循环。。。
此时我心里是万马奔腾的，冷静下来整理思路，操作数组的地方能不用循环的全部干掉。

## Array.prototype

先来复习洗 Array.prototype 的方法

```javascript
 // ES5
 Array.prototype.indexOf
 Array.prototype.lastIndexOf
 Array.prototype.every
 Array.prototype.some
 Array.prototype.forEach
 Array.prototype.map
 Array.prototype.filter
 Array.prototype.reduce
 Array.prototype.reduceRight
```
- indexOf

返回在该数组中第一个找到的元素位置，如果它不存在则返回-1

- lastindexOf

返回在该数组中最后一个找到的元素位置，和 indexof 相反。

- forEach()

forEach 有一个小缺点：不能使用 `break` 语句来跳出循环，也不能使用 `return` 语句来从闭包函数中返回。

```javascript
var arr = [1,2,3,4,5,6,7,8];

arr.forEach(function(item,index){
console.log(item);
});
```
- map()

对数组的每个元素进行一定操作（映射）后，会返回一个新的数组， map()是处理服务器返回数据时是一个非常实用的函数。

```javascript
var oldArr = [
  {first_name:"Colin",last_name:"Toh"},
  {first_name:"Addy",last_name:"Osmani"},
  {first_name:"Yehuda",last_name:"Katz"}];

function getNewArr(){
  return oldArr.map(function(item,index){
    item.full_name = [item.first_name,item.last_name].join(" ");
    return item;
  });

}
console.log(getNewArr());
```
说道map，前两天看到过一个面试题

> 不用循环，创建一个包含从0到99(n)的连续整数的数组

一般这样写

```javascript
var arr = Array(100).join(' ').split('').map(function(item,index){return index});
```
Array(100) 创建了一个包含100个 undefined 的数组,但是这样的数组是没法迭代的(参考 forEach 方法的 定义 )，所以要通过字符串转换，覆盖 undefined ，最后调用 map 修改元素值。

有了 es6 ,用 Array.from 的写法是这样的

```javascript
var arr = Array.from({length:100}).map(function(item,index){return index});
```
Array.from({length:100}) 也是创建了一个包含100个 undefined 的数组，但是这个数组可以迭代`( [].slice.call({length:100})`创建的不可迭代 )，可以直接调用 map 方法。

不过在性能上却有些尴尬。直接 for 循环性能最好，第二种次之，Array.from 最差。

- every()

检测数组中的每一项是否符合条件

```javascript
var ary = [12,23,24,42,1];
var result = ary.every(function(item, index){
  return item > 0
})
console.log(result)
```

- some()

检测数组中是否有某一项符合条件

- for-in
for-in 设计的目的是用于遍历包含键值对的对象，对数组并不是那么友好。
for-in 中 `index` 变量是 "0"、"1"、"3" 等这样的字符串，而并不是数值类型，在某些情况下，上面代码将会以任意顺序去遍历数组元素，太可怕。

- filter()

filter() 方法创建一个新的匹配过滤条件的数组。

```javascript
// 比如找 "apple"
var heroes = [
  {"name":"apple", "count": 2},
  {"name":"orange", "count": 5},
  {"name":"pear", "count": 3},
  {"name":"orange", "count": 16},
];
function isBlackWidow(hero) {
    return (hero.name === 'apple');
}

var blackWidow = heroes.filter(isBlackWidow)[0];

// 这段代码的问题是效率不够高,找到之后不会停止
// 我们可以写一个 find 函数来返回第一次匹配上的元素
// for of 是 ES6 引入新的循环遍历语法接下来会说道
function find(predicate, arr) {
    for (let item of arr) {
        if (predicate(item)) {
            return item;
        }
    }
}

var blackWidow = find(isBlackWidow, heroes);

// JavaScript 已经提供了这样的方法：
var blackWidow = heroes.find(isBlackWidow);
```

- for-of

它是遍历数组最简单直接的方法，也可以用于遍历字符串,会正确识别32位 UTF-16字符等等特性与 `forEach()` 不同的是，它支持 `break`、`continue` 和 `return` 语句。

它还适用于 `Map` 和 `Set` 对象(Map 和 Set 是ES6中的新对象)。

```javascript
// 例如，可以用一个 Set 对象来对数组元素去重
var uniqueWords = new Set(words);

// 当得到一个 Set 对象后去遍历该对象
for (var word of uniqueWords) {
  console.log(word);
}

```
`for-of` 不能直接用来遍历对象的属性,如果你想遍历对象的属性，你可以使用 `for-in`

- Iterator

Iterator（遍历器）就是一种机制；任何数据结构只要是部署了 iterator 接口，就可以完成遍历操作

> Iterator的作用有三个：一是为各种数据结构，提供一个统一的、简便的访问接口；二是使得数据结构的成员能够按某种次序排列；三是ES6创造了一种新的遍历命令for...of循环，Iterator接口主要供for...of消费。（阮一峰）；

只要是一个对象部署了 Symbol.interator 接口，就可以用 for...of 遍历该对象，同时也可以调用该接口的 Symbol.interator 方法调用 next() 方法对对象进行遍历，不同的是 for..of 是对该对象的值的输出，next() 返回的是对象。

```javascript
var arr = ['a','b'];
for (variable of arr) {
  console.log(variable) // a b
}

vat it = arr[Symbol.iterator]();
console.log(it.next()); // {value:'a';done:false}
console.log(it.next()); // {value:'b';done:false}
```
