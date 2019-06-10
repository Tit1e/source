---
title: 网页打印指定区域内容
date: 2019-03-26 22:26:32
tags: ['学习']
---

公司业务上有打印需求，但打印内容在一个弹窗中，在网上找了一圈，找到了解决办法。

```javascript
let dom = document.getElementById('content')
let win = window.open('')
win.document.write(dom.outerHTML)
win.print()
```

以上代码就实现了指定区域的打印功能。

[参考文章](https://www.zhihu.com/question/20108278/answer/14142503)

