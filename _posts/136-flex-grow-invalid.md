---
title: flex-grow 属性无效，宽度被撑开问题解决方法
date: 2020-11-29 20:02:18
tags: ['学习']
---

在使用 flex 布局的时候，有时会遇到 flex-grow 属性无效的情况，期望是两个子元素宽度平均分，但是实际情况就是无法平均分布，设置 `flex-shrink: 0;` 也无效，我用文字超出不换行来复现这种情况。

```css
#flex-box{
  display: flex;
  border: 1px solid red;
  padding: 8px;
}
#flex-box div{
  flex-grow: 1;
  border: 1px solid black;
}
#flex-box div p{
  white-space: nowrap;
}
```

```html
<div id="flex-box">
  <div>
    <p>超出不换行</p>
  </div>
  <div>
    <p>超出不换行，超出不换行，超出不换行，超出不换行，超出不换行，超出不换行，超出不换行</p>
  </div>
</div>
```

上述代码渲染出的 dom 是这个样子的：

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2020-11-29-120940.png)

明明对 div 元素设置了 `flex-grow: 1;` 属性，但是，实际上两个 div 的宽度并没有被平均分。

解决办法很简单，只要把 div 的 width 属性设置为 0px，这样 div 的宽度就是由 `flex-grow: 1;` 来决定的了。

```css
#flex-box{
  display: flex;
  border: 1px solid red;
  padding: 8px;
}
#flex-box div{
  /* 加上这一行 css */
  width: 0px;
  flex-grow: 1;
  border: 1px solid black;
}
#flex-box div p{
  white-space: nowrap;
}
```

把宽度设置为 0 后，浏览器中渲染出的效果就是我们想要的效果了。

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2020-11-29-121707.png)

同理，如果设置了 `flex-direction: column;` ，那就需要把 height 属性设置为0px。