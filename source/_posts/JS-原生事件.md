---
title: JS 原生事件
date: 2017-09-20 22:12:00
tags:
---
# JS原生事件

事件流，所谓的事件流就是页面接受事件的顺序。IE 的事件流叫冒泡事件，Netscape 提出了另一个事件流叫事件捕获。事件的三阶段，分别为捕获，目标，冒泡。

- 捕获阶段：从最上层元素，一直到最下层，你点击的那个 target 元素，路过的所有节点都可以捕捉到这事件
- 目标阶段：如果给事件成功的到达了 target 元素，它会进行事件处理
- 冒泡阶段：事件从最下层向上传递，依次出发父元素的该事件处理函数

阻止事件冒泡，可以使用 `e.stopPropagation()`（Firefox，chrome）或者 `e.cancelBubble=true`（IE）来阻止事件的冒泡传播。

#### 事件绑定

直接在 DOM 元素上绑定 onclick、onmouseover、onmouseout、onmousedown、onmouseup、ondblclick、onkeydown、onkeypress、onkeyup 等事件,缺点是只能实现一个绑定，也就是说我们再为 element 绑定第二个 click 事件时候，会覆盖掉之前的 click 事件

```javascript
// 在 html 中直接写
<button onclick="alert('你点击了这个按钮');"> 点击这个按钮 </button>  

// DOM 绑定
var node = document.getElementById('div')
node.onclick = function() {
    console.log('click 事件绑定成功')
}
```

#### 事件监听

事件监听实现的功能和直接绑定差不多新增了一个特点。无论监听多少次，都不会覆盖掉前面的监听事件。本质原因是监听事件每次都会生产一个全新的匿名函数，和前面的函数完全不同。     
原生的事件绑定是这样的 `btn.addEventListener(“click”,function(event){}, false)`        
IE 浏览器提供的是 `btn.attachEvent(“click”,function(event){}, false)`      
`true` 表示捕获阶段调用事件处理程序             
`false` 表示冒泡阶段调用事件处理程序      
相应的移除事件绑定这是 `removeEventListener` 和 `detachEvent` 

```javascript
var div = document.getElementById('div')
div.addEventListener('click', function() {
    console.log('事件绑定成功1')
})
div.addEventListener('click', function() {
    console.log('事件绑定成功2')
})
```

#### 事件委托

事件委托是基于上面利用了事件冒泡，只指定一个事件处理程序，优点减少监听器,监听动态内容。

```javascript
// html
<ul class="list">
    <li></li>
</ul>
// js
var ul = document.querySelector('.list')
list.addEventListener('click',function (e){
    var l = e.target
    while(l.tagName !== 'LI'){
        l = l.parentNode
        if(l === ul){
            l = null
            break;
        }
    }
    if (l) {
        console.log('执行回调')
    }
})

```

#### DOM3 级事件

DOM0 就是直接通过 onclick 写在 html 里面的事件       
DOM2 是通过 addEventListener 绑定的事件,  IE 下的 DOM2 事件通过 attachEvent 绑定            
DOM3 是一些新的事件     

判定浏览器是否支持该事件类型
```javascript
var isSupported=document.implementation.hasFeature('HTMLEvents','2.0');
alert(isSupported);//true or false
var isSupported=document.implementation.hasFeature("UIEvent",'3.0');
alert(isSupported);//true or false
```

第 DOM3 级事件

- UI 事件：当用户与页面上的元素交互时除法     
    load, unload,abord,error,select,resize,scroll
- 焦点事件：元素获得或失去焦点        
    blur、DOMfocusOut、focusout 是事件目标是失去焦点的元素
    focus、DOMfocusIn、focusin 是事件获取到焦点的元素
- 鼠标事件：通过鼠标在页面上执行操作     
    click，dbclick,mousedown，mouseenter、mouseleave、mousemove、mouseout、mouseover、mouseup
- 滚轮事件：使用鼠标滚轮或类似设备          
    mousewheel，即当用户通过鼠标滚轮与页面交互的时候会触发，最终会冒泡到 document 或者 window 上，它包含的 event 对象包含鼠标事件的所有标准信息外，还包含一个特殊的 wheelData 属性，wheelDelta 是 120 的倍数，向前滚动是 120x，向后滚动是 - 120x
- 文本事件：当用户在文档中输入文本；
- 键盘事件：通过键盘在页面上执行操作         
    1、keydown：用户按下键盘上的任意键时候出发，而且如果按住不放的话就会一直触发，会重复触发这个事件，
    2、keypress：当用户按下键盘上的字符键时触发，
    3、keyup：用户释放键盘上的键时触发
    4、textInput: 是 keypress 的补充，在文本显示给用户之前更容易拦截文本，在文本插入文本框之前会触发该事件，该 event 对象包含一个 data 属性，这个属性是值就是用户输入的字符，event 对象的 inputMethod 属性会判断用户输入到文本框的方式，一共有 0~7 个值，其中常用的就是 0 和 1；

    注：0：表示浏览器不知道怎么输入的，
1：表示用户使用键盘输入的，2 表示用户是粘贴进来的
keyCode 的值可以用来表示键盘值，DOM3 之后就不再包含 charCode 了，新增 key 和 char，兼容：keyIdentifier
- 合成事件：当为 IME（Input Method Editor，输入法编辑器）输入字符时除法；
- 变动事件：底层 DOM 结构发生变化        
    DOM2 级的变动事件能再 DOM 中的某一部分发生变化时触发。
包括：DOMSubtreeModified、DomNodeInserted、DomNodeRemove、DomNodeInsertIntoDocument 等。
1、删除节点：removeChild，replaceChild，目标事件：event.target（被删除的节点）；event.relateNode(相关节点) 会触发 DOMNodeRemoved 事件，会冒泡
2、插入节点：appendChild、repalceChild、insertBefore

HTML5 事件        
- contextmenu 事件        
    冒泡，这个事件的目标是发生用户操作的元素。通常使用 contextmenu 事件来显示自定义的上下文菜单，而使用 onclick 事件处理程序来隐藏该菜单，
- beforeunloaded 事件     
    为了让开发人员有可能在页面卸载前阻止这一操作          
- DOMContent 事件         
    在 window 的 load 事件会在页面中的一切都加载完毕时候触发，但是这个事件在形成 DOM 树的时候就会触发，这个事件会冒泡到 window 但它的目标实际上是 document，它不会提供任何额外的信息（其 target 属性是 document）           
- pageShow、pagehide(Firefox,Opera)        
    pageShow 会在 load 事件触发后触发，当页面离开并且点击返回时候，返回到上一个页面的时候会触发，event 事件的 persisted 为 true 的时候页面会保存在 bfchache 中，
pagehide 事件，该事件会在浏览器卸载的时候触发，会在 unload 事件之前触发，和 pageShow 一样在 document 上触发        
- haschange 事件          
    当 URL 的参数列表发生变化的时候触发，（# 后面的所有字符）

