---
title: DOM编程总结
date: 2016-08-02 22:48:59
tags:
---
## 节点操作
### 获取节点
* 父子关系
    * parentNode
    * firstChild/lastChild/childNodes
    * childNodes/children
* 兄弟关系
    * previousSibling/nextSibling
    * previousElementSibling/nextElementSibling
* 选择器   
    * element=document.getElementById(id)
    * collection=element.getElementByTagName(tarName)
        * collection 是个动态的随着节点改变而改变
        * tarName=*代表获取所有
    * collection=element.getElementByClassName(className)
    * list=element.querySelect/All(selector)
* 现代浏览器中内置的可以等效替代jQuery的功能
    - 创建全局的 '$' 变量
    
        ```
          window.$ = function(selector) {   
            return document.querySelector(selector);
          };
        ```

### 创建节点
* element=document.createElement(tagName)

### 修改节点
* element.textContent 获取节点以及后面的文本内容
* element.textContent = "修改内容"
* element.innerText 不规范不推荐使用

### 插入节点
* element.appendChild(a) 在指定元素后追加a
* element.insertBefore(h,a) 在指定元素h之前添加a

### 删除节点
* element.removeChild

### innerHtml
* 节点的HTML内容

## 设置样式
* element.style.cssText='color:red;' 一条语句设置样式
* element.className ='clasname' 编辑类名一次修改多个样式
* element.style.color 获取颜色样式 不一定是它实际样式 当元素上有设置颜色的才获取到实际的
        * window.getComputedStyle(element).color 获取到的是实际

## 事件
### 事件注册
* elem.addEventListener(type,function,false/true)

### 浏览器兼容型事件注册
```
    function addEvent(element,type,handler){
      if (element.addEventListener) {
        element.addEventListener(type,handler,false);
      }else if (element.attachEvent) {
        element.attachEvent('on'+type,handler);
      }else{
        element['on'+type]=handler;
      }
    }
```

### 取消事件注册
* elem.removeEventListener(type,function,false/true)

### 事件触发
* elem.dispatchEvent(type)

### 事件对象
* 属性
    * type
    * target 
       
        ```

            //兼容        
            var event=e || window.event;         
            var target=event.target || event.srcElement; 

        ```

    * currentTarget
* 方法
    * stopPropagation（W3C） 阻止事件传播 cancelBubble（IE低版本）
    * preventDefault（W3C） 阻止默认行为  returnValue=false（IE低版本）
    * stopImmediatePropagation

### 事件类型
* Event
    - load
    - unload
    - error
    - select
    - abort
* UIEvent   //继承自Event
    - resize
    - scroll
* FocusEvent    //继承自UIevent
    - blur
    - focus
    - focusin
    - focusout
* InputEvent    //继承自UIevent
    - beforeinput
    - input
* KeyboarEvent  //继承自UIevent
    - keydown
    - keyup
* MouseEvent    //继承自UIevent
    - click
    - dbclick
    - mousedown
    - mouseleave
    - mousemove
    - mouseout
    - mouseup
* wheelEvent    //继承自MouseEvent
    - wheel



