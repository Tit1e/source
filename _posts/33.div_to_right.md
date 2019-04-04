---
title: 块级元素靠右的实现方式
date: 2017-10-12 21:07:56
tags: ['学习笔记']
---
一个块级元素靠右的实现方式有那些呢? 今天从别人哪里刚刚又学到了点东西, 整理了一下:
```html
<!-- 基本结构 -->
<div id="parent">
  <div id="block"></div>
</div>
```
<!-- more -->
#### 使用 margin 属性

```css
#block{
  margin-left: auto;
}
```
将 `margin-left` 设为 `auto` 后, 元素左边的 `margin` 会被尽可能的撑大, 所以自然就把元素挤到右边去了. 

#### 使用 position 属性
```css
#parent{
  position: relative;
}
#block{
  position: absolute;
  right: 0;
}
```
使用定位, 绝对能把元素放到右边去. 

#### 使用 float 属性
```css
#block{
  float: right;
}
```
使用浮动将元素浮动到右边. 

#### 使用 text-align 属性
```css
#parent{
  text-align: right;
}
#block{
  display: inline-block;
}
```
将块设为行内元素,然后父元素使用 `text-align: right;` 是块到右边. 

#### 使用 flex 属性
```css
#parent{
  display: flex;
  justify-content: flex-end;
}
```
将父元素变为弹性容器, 然后将 `justify-content` 设为 `flex-end`, 那么容器中的弹性元素会从右开始排列. 

以上就是我整理块级元素靠右排列的几种方式, 如有遗漏, 欢迎补充. 