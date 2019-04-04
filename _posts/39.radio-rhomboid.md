---
title: css 实现圆角平行四边形
date: 2017-12-17 21:31:07
tags: ['学习笔记', 'css']
---
公司管理平台首页改版中有个圆角平行四边形的标签页。由于以前没接触到过这个，没什么经验，所以我只好求助万能的 google ，但是我用关键字“css 圆角平行四边形”并没有搜出答案来，于是我退而求其次，那就搜“css 平行四边形”，这个我有把握 100% 能用 强大的css 实现，果然一搜就出现了：[CSS秘密花园：平行四边形](https://www.w3cplus.com/css3/css-secrets/parallelograms.html)。
<!--more-->
实现代码：
```css
.button{
  transform: skewX(-30deg);
  background: linear-gradient(to right, #7BBBB2, #46AEAA); 
  color: #fff;
  text-align: center;
  padding: 8px 20px;
  display: inline-block;
  border-radius: 4px;
  box-sizing: border-box;
  margin-left: 10px;
  box-shadow: 0px 5px 10px -3px rgba(0, 0, 0, 0.5);
}
.button > div{
  transform: skewX(30deg);
}
```
```html
<div class="button">
  <div>
    首页
  </div>
</div>
<div class="button">
  <div>
    采集管理
  </div>
</div>
<div class="button">
  <div>
    数据管理
  </div>
</div>
```
简单来说就是利用css的`transform: skewX(deg)`属性来实现元素的扭曲，但是元素扭曲的同时，会使元素内的内容也随之扭曲，所以就需要用另一个元素把不想扭曲的元素包裹起来，然后对外层使用扭曲，内层元素再使用反向扭曲来抵消，一次来实现效果。然后我又给外层元素添加了`border-radios`，于是圆角平行四边形就诞生啦！

效果图：
![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2018-05-10-144722.png)
