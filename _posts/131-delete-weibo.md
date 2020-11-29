---
title: 批量删除微博，可自定义删除内容
date: 2020-05-23 15:36:40
tags: ['折腾']
---

首先浏览器上登录微博，浏览器要求： QQ浏览器，360极速浏览器，谷歌浏览器，火狐浏览器等都行，可以先试试，不行再换。Safari我记得默认是关闭调试模式的，

Windows 用户直接按 F12 打开控制台，或者在网页上右键，会有【元素审查】、【检查】类似这种的菜单项，打开它，正常情况浏览器内会出现一个窗口。

然后点击【console】标签页，复制代码，在箭头在的位置粘贴代码，如果你要按条件删除，那就先编辑好条件，然后按回车。

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2020-05-23-082341.png)

根据实际使用来说，删除要等个十来秒才会执行，当页面在闪的时候说明在删除了。

使用的时候自动跳到下一页好像有点问题。如果页面长时间没反应，那就刷新一下页面，再粘贴执行一遍代码。

这是按条件删除微博的代码：

```js
'use strict';
var s = document.createElement('script')
s.setAttribute(
  'src',
  'https://lib.sinaapp.com/js/jquery/2.0.3/jquery-2.0.3.min.js'
)
s.onload = function() {
  let index = 0
  setInterval(function() {
    if (!$('div[action-type="feed_list_item"]').length) {
      // 删完符合条件的微博，翻到下一页
      $('a.next').click()
    } else {
      const firstItem = $('div[action-type="feed_list_item"]')[index]
      const text = $(firstItem).find('.WB_feed_detail .WB_detail div[node-type="feed_list_content"]').text()
      // 删除微博正文中含有 “今天” 两个字的微博
      const needDelete = text.indexOf('今天') > 0
      // 如果要匹配多个：
      // 下面这样写会把微博正文中含有 “今天” 或者 “第一” 的微博删除，含有其中之一就会被删
      // const needDelete = text.indexOf('今天') > 0 || text.indexOf('第一') > 0

      // 这样写是将微博正文中含有 “今天” 和 “第一” 的微博删除，必须两个都包含才会被删
      // const needDelete = text.indexOf('今天') > 0 && text.indexOf('第一') > 0

      if(needDelete){
        $(firstItem).find('a[action-type="feed_list_delete"]')[0].click()
        $(firstItem).find('a[action-type="ok"]')[0].click()
      }else{
        index += 1
      }
    }

    // 滚动到底部自动加载
    $('html, body').animate({ scrollTop: $(document).height() }, 'slow')
  }, 800)
}
document.head.appendChild(s)
```

这是删除所有微博的代码：[来源](https://greasyfork.org/en/scripts/14709-weibored-js/code)

```js
'use strict';

var s = document.createElement('script');
s.setAttribute(
  'src',
  'https://lib.sinaapp.com/js/jquery/2.0.3/jquery-2.0.3.min.js'
);
s.onload = function() {
  setInterval(function() {
    if (!$('a[action-type="feed_list_delete"]')) {
      $('a.next').click();
    } else {
      $('a[action-type="feed_list_delete"]')[0].click();
      $('a[action-type="ok"]')[0].click();
    }

    // scroll bottom let auto load
    $('html, body').animate({ scrollTop: $(document).height() }, 'slow');
  }, 800);
};
document.head.appendChild(s);
```



