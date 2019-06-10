---
title: 用table、flex、float、position等实现一些元素、页面的布局
date: 2016-08-30
tags: ['学习']
---
整理了一些布局的实现方案，算是笔记，发到博客上也希望能帮助到一些有需要的人。

注：文中列出的只是核心代码，在实际应用中可能还需加入"width"/"height"/"bancground-color"等css样式。
<!--more-->
# 居中布局

## 元素水平居中
之后的布局讲解将以下面的这段代码为模板
```html
<div class="parent">
    <div class="child">DEMO</div>
</div>
```
### inline-block + text-align
```css
.child{
    display: inline-block;
    text-align: left;
}
.parent{
    text-align: center;
}
/*
优点：兼容性较好，可以兼容到IE6、IE7，但IE6、IE7不支持inline-block，需要用其他方法实现IE6/7中的inline-block。
缺点：如果不想文字水平居中需单独设置child的text-align，因此会产生额外的代码来修复text-align:center造成的一些问题。
*/
```
### table + margin
```css
.child{
    display: table;
    margin: 0 auto;
}
/*
优点：只对child设置样式，不影响parent，兼容到IE8级以上浏览器。
缺点：IE6/7不支持display:table属性，但可以通过把<div>结构换成<table>结构来实现兼容。
*/
```
### absolute + transform
```css
.parent{
    position:relative;
}
.child{
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
}
/*
优点：绝对定位脱离文档流，因此居中对象不会对其他元素产生影响
缺点：transform不支持IE6/7/8，因此兼容性较差，在高版本浏览器中可能需要加私有前缀
*/
```
### flex + justify-content
```css
/*方法一*/
.parent{
    display: flex;
    justify-content: center;
}

/*方法二*/
.parent{
    display: flex;
}
.child{
    margin: 0 auto;
}
/*
优点：只对parent元素修改，不影响child的样式
缺点：flex不支持IE6/7/8，因此兼容性较差
*/
```
## 元素的垂直居中

### table-cell + vertical-align
```css
.parent{
    display: table-cell;
    vertical-align: middle;
}
/*
优点：支持IE8及以上版本浏览器
缺点：如果需要支持IE6/7，要将<div>结构换成<table>结构
*/
```
### absolute + transform
```css
.parent{
    position: relative;
}
.child{
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
}
/*
优点：绝对定位脱离文档流，因此居中对象不会对其他元素产生影响
缺点：transform不支持IE6/7/8，因此兼容性较差，在高版本浏览器中可能需要加私有前缀
*/
```
### flex + align-items
```css
.parent{
    display: flex;
    align-items: center;
}
/*
优点：只对parent元素修改，不影响child的样式
缺点：flex不支持IE6/7/8，因此兼容性较差
*/
```
## 元素的水平垂直居中（元素宽高都未知）
### inline-block + text-align + table-cell + vertical-align
```css
.parent{
    text-align: center;
    display: table-cell;
    vertical-align: middle;
}
.child{
    display: inline-block;
}
/*
优点：兼容性较高，支持IE8及以上浏览器
缺点：如果需要支持IE6/7，要将<div>结构换成<table>结构
*/
```
### absolute + transform
```css
.parent{
    position: relative;
}
.child{
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%,-50%)
}
/*
优点：绝对定位脱离文档流，因此居中对象不会对其他元素产生影响
缺点：transform不支持IE6/7/8，因此兼容性较差，在高版本浏览器中可能需要加私有前缀
*/
```
### flex + justify-content + align-items
```css
.parent{
    display: flex;
    justify-content: center;
    align-items:center;
}
/*
优点：只设置parent，不影响child
缺点：兼容性较差，可能只有高版本浏览器能实现
*/
```
# 多列布局

## 一列定宽一列自适应布局
之后的布局讲解将以下面的这段代码为模板,如模版有变动将单独列出。
```html
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
### float + margin
```css
.left{
    float: left;
    width: 100px;
}
.right{
    margin-left: 120px;/*多的20px为列之间的空隙*/
}
/*
优点：容易理解
缺点：兼容性有一点小问题，IE6中，因为left浮动，right不浮动，右边的文字会往右缩进3像素，解决方法：在left的css中加入"margin-right: -100px;"。清除浮动会打乱布局
*/
```
改进上述方法，解决兼容性问题

**此结构只适用于float + margin + (fix)方法**
```html
<div class="parent">
    <div class="left">
        <p>left</p>
    </div>
    <div class="right-fix">
        <div class="right">
            <p>right</p>
            <p>right</p>
        </div>
    </div>
</div>
```
### float + margin + (fix)
```css
.left{
    float: left;
    width: 100px;
    position: relative;
}
.right-fix{
    float: right;
    width: 100%;
    margin-left:-100px;
}
.right{
    margin-left: 120px;
}
/*
优点：兼容IE6及以上所有浏览器
缺点：结构、样式会相对复杂
*/
```
### float + overflow
```css
.left{
    float: left;
    width: 100px;
    margin-right: 20px;/20px为列之间的空隙/
}
.right{
    overflow: hidden;
}
/*
优点：样式简单
缺点：IE6不支持
*/
```
### table
```css
.parent{
    display: table;
    width: 100%;
    table-layout:fixed;/提升渲染速度，布局优先/
}
.left,.right{
    display: table-cell;
}
.left{
    width: 100px;
    padding-right: 20px;/20px为列之间的空隙,table-cell无法设置margin，所以设置padding/
}
/*
优点：兼容IE8及以上浏览器
缺点：代码比较复杂
*/
```
### flex
```css
.parent{
    display: flex;
}
.left{
    width: 100px;
    margin-right: 20px;/20px为列之间的空隙/
}
.right{
    flex: 1;
}
/*
优点：实现比较容易
缺点：低版本浏览器兼容性较差，flex只适合小范围布局，
*/
```
## 两列定宽一列自适应布局
**此结构只适用于下面的例子**
```html
<div class="parent">
    <div class="left">
        <p>left</p>
    </div>
    <div class="center">
    <p>center</p>
    </div>
    <div class="right-fix">
        <div class="right">
            <p>right</p>
            <p>right</p>
        </div>
    </div>
</div>
```
### float + overflow
```css
.left,.center{
    float: left;
    width: 100px;
    margin-right: 20px;/20px为列之间的空隙/
}
.right{
    overflow: hidden;
}
```
## 一列不定宽一列自适应布局
**之后的布局讲解将以下面的这段代码为模板,如模板有变动将单独列出**
```html
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
### float + overflow
```css
.left,.center{
    float: left;
    margin-right: 20px;/*20px为列之间的空隙*/
}
.right{
    overflow: hidden;
}
.left p{
    width:200px;/*这里的宽度可以随意更改,即由内容决定*/
}
/*
优点：代码量少，实现简单
缺点：在IE6下兼容性有问题
*/
```
### table
```css
.parent{
    display: table;
    width: 100%;
}
.left,.right{
    display: table-cell;
}
.left{
    width: 0.1%;/*写成0.1%是出于兼容性考虑，1px在IE8下会有问题*/
    padding-right: 20px;/*20px为列之间的空隙,table-cell无法设置margin，所以设置padding*/
}
.left p{
    width: 200px;/*这里的宽度可以随意更改,即由内容决定*/
}
/*
优点：兼容IE8及以上浏览器
缺点：IE6/7不支持
*/
```
### flex
```css
.parent{
    display: flex;
}
.left{
    margin-right: 20px;/20px为列之间的空隙/
}
.right{
    flex: 1;
}
.left p{
    width: 100px;/根据内容决定/
}
/*
优点：实现比较容易
缺点：低版本浏览器兼容性较差，flex只适合小范围布局
*/
```
## 两列不定宽加一列自适应
**此结构只适用于下面的例子**
```html
<div class="parent">
    <div class="left">
        <p>left</p>
    </div>
    <div class="center">
    <p>center</p>
    </div>
    <div class="right-fix">
        <div class="right">
            <p>right</p>
            <p>right</p>
        </div>
    </div>
</div>
```
### float + overflow
```css
.left,.center{
    float: left;
    width: 100px;
    margin-right: 20px;/*20px为列之间的空隙*/
}
.right{
    overflow: hidden;
}
```
## 等分布局
**之后的布局讲解将以下面的这段代码为模板,如模板有变动将单独列出**
```html
<div class="parent">
    <div class="column"><p>1</p></div>
    <div class="column"><p>2</p></div>
    <div class="column"><p>3</p></div>
    <div class="column"><p>4</p></div>
</div>
```
### float
```css
.parent{
    margin-left: -20px;
}
.column{
    float: left;
    width: 25%;
    padding-left: 20px;
    box-sizing: border-box;/*可以让设置的宽度包含padding*/
}
/*
优点：兼容IE8及以上浏览器
缺点：当列数发生变化时需要修改css样式
*/
```
### table
**此模板只适用于table布局**
```html
<div class="parent-fix">
    <div class="parent">
        <div class="column"><p>1</p></div>
        <div class="column"><p>2</p></div>
        <div class="column"><p>3</p></div>
        <div class="column"><p>4</p></div>
    </div>
</div>
```
```css
.parent-fix{
    margin-left: -20px;
}
.parent{
    display: table;
    width:100%;
    table-layout:fixed;/*布局优先，单元格自动平分*/
}
.column{
    display: table-cell;
    padding-left: 20px;
}
/*
优点：列数改变自动平分
缺点：结构更加复杂
*/
```
### flex
```css
.parent{
    display: flex;
}
.column{
    flex: 1;
}
.column+.column{
/*这个选择器表示选择前一个class为'column'的下一个'column'*/
    margin-left: 20px;
}
/*
优点：代码量少，自动平分
缺点：兼容性较差
*/
```
## 等高布局
**之后的布局讲解将以下面的这段代码为模板,如模板有变动将单独列出**
```html
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
### table
```css
.parent{
    display: table;
    width: 100%;
    table-layout:fixed;/*提升渲染速度，布局优先*/
}
.left,.right{
    display: table-cell;
}
.left{
    width: 100px;
    padding-right: 20px;/*20px为列之间的空隙,table-cell无法设置margin，所以设置padding*/
}
/*
背景颜色默认会覆盖在padding区域内，使空隙无法看出，需设置background-clip:content-box;使背景颜色只显示在内容区域内
*/
```
### flex(flex布局默认等高)
```css
.parent{
    display: flex;
}
.left{
    margin-right: 20px;/*20px为列之间的空隙*/
}
.right{
    flex: 1;
}
.left p{
    width: 100px;/*根据内容决定*/
}
```
### float
```css
.parent{
    overflow: hidden;
}
.left,.right{
    padding-bottom: 9999px;
    margin-bottom: -9999px;
}
.left{
    float: left;
    width: 100px;
    margin-right;
}
.right{
    overflow: hidden;
}
/*
优点：兼容性好
缺点：伪等高，视觉上等高，实际不等高
*/
```
## 全屏布局
**之后的布局讲解将以下面的这段代码为模板,如模板有变动将单独列出**
```html
<div id="parent">
    <div id="top"></div>
    <div id="left"></div>
    <div id="right"></div>
    <div id="bottom"></div>
</div>
```
### position(scroll)
```css
html,body,#parent{
    height:100%;
    overflow:hidden;
}
.top{
    position:absolute;top:0;left:0;right:0;height:100px;
}
.bottom{
    position:absolute;left:0;right:0;bottom:0;height:50px;
}
.left{
    position:absolute;left:0;top:100px;bottom:50px;width:200px;
}
.right{
    position:absolute;overflow:auto;left:200px;top:100px;bottom:50px;
}
/*
IE6不支持position，如需百分比显示布局的大小，只需将代码中的具体像素改为百分比
*/
```
### flex
**此模板只适用于该案例**
```html
<div class="parent">
    <div class="top"></div>
    <div class="middle">
        <div class="left"></div>
        <div class="right"></div>
    </div>
    <div class="bottom"></div>
</div>
```
```css
html,body,.parent{
    height:100%;
    overflow:hidden;
    margin:0;
}
.parent{
    display:flex;
    flex-direction:column;
}
.top{
    height:100px;
    background-color:black;
}
.bottom{
    height:50px;
}
.middle{
    flex-grow:1;
    display:flex;
}
.left{
    width:200px;
}
.right{
    flex-grow:1;
    overflow:auto;
}
/*
IE9及以下浏览器不兼容，如需百分比显示布局的大小，只需将代码中的具体像素改为百分比
*/
```
## 全适应布局
position无法实现全适应布局，只能用flex或grid实现，这里只介绍flex实现方法。

**此模板只适用于该案例**
```html
<div class="parent">
    <div class="top"></div>
    <div class="middle">
        <div class="left"></div>
        <div class="right"></div>
    </div>
    <div class="bottom"></div>
</div>
```
```css
html,body,.parent{
    height:100%;
    overflow:hidden;
}
.parent{
    display:flex;
    flex-direction:column;
}
.middle{
    flex-grow:1;
    display:flex;
}
.right{
    flex-grow:1;
    overflow:auto;
}
/*
IE9及以下浏览器不兼容
*/
```