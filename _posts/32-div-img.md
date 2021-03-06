---
title: div 包裹 img 底部出现空隙问题
date: 2017-10-12 19:33:53
tags: ['学习']
---
工作中我们有时候会遇到一个 div 包裹着一个 img 的时候, img 底部和 div 之间会有空隙, 如下图所示: 

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2018-05-10-144347.png)

<!-- more -->

页面代码如下: 
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
  <title>Document</title>
</head>
<body style="background-color:blueviolet">
  <div style="background-color:#000;color:#fff;">
    <img src="./2.png">
  </div>
    <script>

    </script>
</body>
</html>
```
这是什么造成的呢? 这样看也许你看不出来, 但是像下面这样也许你就大概知道是为什么了:


![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2018-05-10-144403.png)

没错, 就是因为 `<img>` 不是块级元素,所以在一个 DIV 中它相当于一个文字, 而文字是有 baseline 的, 英文四线格中, baseline 处于倒数第二条而不是最后一条, 所以在图像底部才会出现一段空隙. 所以可以推测出: 在文字变大的时候, 空隙也会相应变大:

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2018-05-10-144416.png)

在<a href="https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/img"> MDN </a>中有这样一段文字来描述 `<img>` 标签: 

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2018-05-10-144435.png)

那么什么是替换元素呢? 

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2018-05-10-144447.png)

也就是说 `<img>` 是一个可以设置款高的行内元素, 是不是和 css 属性中的 `display: inline-block` 有点像呢? 不过 `<img>` 可是一个实打实的 **行内元素** (第 3 排第 4 个):

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2018-05-10-144508.png)

到这里已经完全了解了问题产生的原因, 那么相应解决问题的方法也就比较好找了:

1. 设置图片的 `vertical-align` 属性为 `bottom / top / middle` , 这样 `<img>` 的 baseline 就是最下面的线了,  也就不会产生空隙了. 
2. 既然因为它是替换元素的缘故, 那把它转为块级元素不就好了吗? 所以设置`display: block` 也可以解决这个问题.
3. 把父元素的高度直接设置为图片想要的高度, 如果这个高度不是图片默认的高度, 再把图片的高度设为`height: 100%` 也可以解决这个问题. 

效果如图: 

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2018-05-10-144519.png)