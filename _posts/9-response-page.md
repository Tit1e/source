---
title: 响应式页面的简单实现
date: 2016-08-31
tags: ['学习']
---
## 响应式网页设计
>响应式网页设计（英语：Responsive web design，通常缩写为RWD），或称自适应网页设计、回应式网页设计、对应式网页设计。是一种网页设计的技术做法，该设计可使网站在多种浏览设备（从桌面电脑显示器到移动电话或其他移动产品设备）上阅读和导航，同时减少缩放、平移和滚动。——摘自维基百科
<!--more-->
## 响应式网站的简单实现
```html
<meta name="viewport" content="
    width=device-width, 
    initial-scale=1.0,
    user-scalable=no"
/>
```
上述代码是对`viewport`的设置，`<meta>`标签嵌套在`<head>`标签中。

`width=device-width` 属性是设置页面宽度为设备宽度。

`initial-scale=1.0` 属性是设置页面缩放比例为1.0，即正常大小。

`user-scalable=no` 属性是设置禁止用户手动缩放页面大小，因为响应式网站一般在不同设备都能正常显示网站内容，所以一般不需要用户手动缩放。

```css
/*设备屏幕宽度大于所设的最小宽度时，使用该样式*/
@media screen and (min-width:){
    /*CSS样式*/
}
/*设备屏幕宽度小于于所设的最大宽度时，使用该样式*/
@media screen and (max-width:){
    /*CSS样式*/
}
/*设备屏幕宽度在所设宽度区间内时，使用该样式*/
@media screen and (min-width:) and (max-width:){
    /*CSS样式*/
}
```
注：`@media`需写在CSS样式中，并且要放在一般显示样式之后，只有当一般显示样式不适配的时候才会匹配`@media`中的样式。
