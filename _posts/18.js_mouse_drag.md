---
title: 原生Javascript实现鼠标拖拽div
date: 2017-02-02 20:38:03
tags: ['学习笔记']
---
鼠标拖拽是一种常见的js效果，下面贴一下原生js的实现方法。

注：第一段段js代码来自网络，第二段是在第一段基础上改进实现的。由于时间太久找不到出处，侵删。

先贴上一张对象和窗口之间距离的示意图方便理解，下文中也有注释。
![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2018-12-15-7f157632-e98c-11e6-9d76-79e0ffb08e6c.gif)
<!-- more -->
### HTML代码
``` html
<div id="move"></div>
```
### CSS代码
``` css
html,body {
    height: 100%;
}

body {
    position: relative;
    overflow: hidden;
    margin: 0;
}

#move {
    width: 200px;
    height: 100px;
    background-color: #ddd;
    border: 1px solid #000;
    position: absolute;
    z-index: 1;
}
```
### Javascript代码
#### 可以将元素拖拽出窗口外

``` javascript
// clientX 设置或获取鼠标指针位置相对于当前窗口的 x 坐标，其中客户区域不包括窗口自身的控件和滚动条。 
// clientY 设置或获取鼠标指针位置相对于当前窗口的 y 坐标，其中客户区域不包括窗口自身的控件和滚动条。 
// offsetX 设置或获取鼠标指针位置相对于触发事件的对象的 x 坐标。 
// offsetY 设置或获取鼠标指针位置相对于触发事件的对象的 y 坐标。 
// screenX 设置或获取获取鼠标指针位置相对于用户屏幕的 x 坐标。 
// screenY 设置或获取鼠标指针位置相对于用户屏幕的 y 坐标。 
// x 设置或获取鼠标指针位置相对于父文档的 x 像素坐标(亦即相对于当前窗口)。 
// y 设置或获取鼠标指针位置相对于父文档的 y 像素坐标(亦即相对于当前窗口)。
window.onload = function() {
    var box = document.getElementById("move");
    move.onmousedown = function(e) {
        var e = e || event;
        console.log(e);
        var target = e.target || e.srcElement;
        var disX = e.clientX - target.offsetLeft;
        var disY = e.clientY - target.offsetTop;
        document.onmousemove = function(e) {
            target.style.left = e.clientX - disX + "px";
            target.style.top = e.clientY - disY + "px";
        }
        document.onmouseup = function(e) {
            document.onmousemove = null;
            document.onmousedown = null;
        }
    }
}
```

#### 无法将元素拖拽出窗口

``` javascript
window.onload = function() {
    var box = document.getElementById("move");
    move.onmousedown = function(e) {
        var e = e || event;
        var target = e.target || e.srcElement;
        var disX = e.clientX - target.offsetLeft;
        var disY = e.clientY - target.offsetTop;
        document.onmousemove = function(e) {
            target.style.left = e.clientX - disX + "px";
            target.style.top = e.clientY - disY + "px";
            if (target.offsetLeft + target.offsetWidth >= document.body.clientWidth) {
                target.style.left = document.body.clientWidth - target.offsetWidth + "px";
            }
            if (target.offsetLeft < 0) {
                target.style.left = 0 + "px";
            }
            if (target.offsetTop + target.offsetHeight >= document.body.clientHeight) {
                target.style.top = document.body.clientHeight - target.offsetHeight + "px";
            }
            if (target.offsetTop < 0) {
                target.style.top = 0 + "px";
            }
        };
        document.onmouseup = function(e) {
            document.onmousemove = null;
            document.onmousedown = null;
        }
    }
}
```
