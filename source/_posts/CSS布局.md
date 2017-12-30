---
title: CSS布局
date: 2016-07-27 23:42:41
tags:
    - CSS
    - 布局
    - 居中
---
## CSS布局
### 布局--display
#### block
* 默认宽度是父元素宽度
* 可设置宽高
* 换行显示
* 常见块状元素 div p h1-h6 ul from

#### inline
* 默认宽度是内容宽度
* 不可设置宽高
* 同行显示
* 常见行内元素 span a label cite em

#### inline-block
* 默认宽度是内容宽度
* 可设置宽高
* 同行显示
* 常见的行内块状元素 input textarea select button

#### none
* 隐藏不占位置
* visiblity：hidden隐藏占位置

### 布局--position
#### relative
* 在文档流中
* 参照物是本身

#### absolute
* 默认宽度是内容宽度
* 脱离文档流
* 参照物是第一个定位的祖先元素

#### fixed
* 默认宽度是内容宽度
* 脱离文档流
* 参照物是视窗

### 布局--float
* 默认宽度为内容宽度
* 半脱离文档流，对元素脱离文档流，对内容在文档流
* 向指定方向移动   

#### 清除浮动
```
.clearfix:before,.clearfix:after{
    content: "";
    display: table;
}
.clearfix:after{
    clear: both;
    overflow: hidden;
}
.clearfix{
    zoom:1; /*for IE6 IE7*/
}
```
### 居中布局
#### 水平居中
```
<div class="parent">
    <div class="child">DEMO</div>
</div>
body{margin:20px;}
```
* inline-block + text-align

        优点：兼容性好；缺点：子元素会继承父元素的居中
```  
.parent{
    text-align: center;
}
.child{
    display: inline-block;
}
```
* table + margin
```
.child{
    display: table;
    margin: 0 auto;
}
```
* absolute + transform

        优点：脱离文档流对其它元素没有影响。缺点：兼容性问题
```
.parent{
    height:1.5em;
    position: relative;
    }
.child{
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
}
```
* flex + justify-content
```
.parent{
    display: flex;
    justify-content: center;
}
```
#### 垂直居中
```
<div class="parent">
    <div class="child">DEMO</div>
</div>
body{margin:20px;}
    .parent{width:4em;height:500px;}
    .child{width:100%;}

```
* table-cell + vertical-align
```
.parent{
    display: table-cell;
    vertical-align: middle;
}

```
* absolute + transform
```
.parent{
    position: relative;
}
.child{
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
}

```
* flex + align-items
```
.parent{
    display: flex;
    align-items: center;
}

```

<iframe width="100%" height="300" src="//jsfiddle.net/lennonover/u9erhetx/embedded/html,css,result/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

#### 垂直水平居中
```
<div class="parent">
    <div class="child">DEMO</div>
</div>

body{margin:20px;}
.parent{width:200px;height:300px;}
```
* inline-block + text-aligin + table-cell + vertical-align
```
.parent{
    text-align: center;
    display: table-cell;
    vertical-align: middle;
}
.child{
    display: inline-block;
}
```
* absolute + transform
```
.parent{
    position: relative;
}
.child{
    position: absolute;
    left: 50%;
    top: 50%;
    transform:
    translate(-50%,-50%);
}
```
* flex + justify-content + align-items
```
.parent{
    display: flex;
    justify-content: center;
    align-items: center;
}
```
### 多列布局
#### 定宽自适应
```
<div class="parent">
    <div class="left">
        <p>left</p>
    </div>
    <div class="right">
        <p>right</p>
        <p>right</p>
    </div>
</div>
```
##### 左侧定宽右侧自适应
* float + margin
```
.left{
    float: left;
    width: 100px;
}
.right{
    margin-left: 120px;
}
//在IE6中因为right是没有浮动的所以right里面的文字会往右边缩进3像素
```
* float + overflow
```
.left{
    float: left;
    width: 100px;
    margin-right: 20px;
}
.right{
    overflow: hidden;
}
//IE6不支持
```
* table

        table默认宽度内容宽度、且对margin无效、响应padding
```
.parent{
    display: table;
    width: 100%;
    table-layout: fixed;//布局优先、且加速table渲染
}
.left,.right{
    display: table-cell;
}
.left{
    width: 100px;
    padding-right: 20px;
}
```
* flex
```
.parent{
    display: flex;
}
.left{
    width: 100px;
    margin-right: 20px;
}
.right{
    flex: 1;
}
```

##### 左侧不定宽右侧自适应
* float + overflow
* table
* flex

#### 等分布局
* float
```
<div class="parent">
    <div class="column"><p>1</p></div>
    <div class="column"><p>2</p></div>
    <div class="column"><p>3</p></div>
    <div class="column"><p>4</p></div>
</div>
.parent{
    margin-left: -20px;
}
.column{
    float: left;
    width: 25%;
    padding-left: 20px;
    box-sizing: border-box;
}
```
* table
```
<div class="parent-fix">
    <div class="parent">
        <div class="column"><p>1</p></div>
        <div class="column"><p>2</p></div>
        <div class="column"><p>3</p></div>
        <div class="column"><p>4</p></div>
    </div>
</div>
.parent-fix{
    margin-left: -20px;
}
.parent{
    display: table;
    width:100%;
    table-layout: fixed;//平分单元格
}
.column{
    display: table-cell;
    padding-left: 20px;
}
```
* flex
```
<div class="parent">
    <div class="column"><p>1</p></div>
    <div class="column"><p>2</p></div>
    <div class="column"><p>3</p></div>
    <div class="column"><p>4</p></div>
</div>
.parent{
    display: -flex;
}
.column{
   flex:1;
}
.column+.column{
    margin-left:20px;
}
```

#### 等高布局
* table
* flex
* float
```
//float的伪等高
<div class="parent">
    <div class="left">
        <p>left</p>
    </div>
    <div class="right">
        <p>right</p>
        <p>right</p>
    </div>
</div>
body{margin:20px;}
p{background: none!important;}
.left,.right{background: #444;}
.parent{
    overflow: hidden;
}
.left,.right{
    padding-bottom: 9999px;
    margin-bottom: -9999px;
}
.left{
    float: left; width: 100px;
    margin-right: 20px;
}
.right{
    overflow: hidden;
}
```
