---
title: 浅谈 localStorage 和 sessionStorage
date: 2018-04-26 20:52:47
tags: ['学习']
---
说到前端的本地存储，一般人都会想到 localStorage、sessionStorage 和 Cookie。但这里我暂且不谈 cookie，来说说这两个长得比较像的。
<!--more-->
`localStorage` 是 HTML5 的新特性，用于数据的存储，但是不同于 `sessionStorage`，它的存储期限可以是无期限，只要用户不手动去清除它。而 `sessionStorage` 则在页面被关闭时就会被清除。它们可以存储的大小都为 5M。而 `Cookie` 则如它的名字，存储空间也只有小小的 4kb。

我们先来看下 `localStroage` 是个什么东西，在 chrome 的控制台直接打印，我门可以看到这些东西：

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2018-05-10-144749.png)

我们可以直观地看到 `localStroage` 是一个对象，并且在有一个 length 属性，先猜测它是用来表示键值对数量的，并且我们可以看到 `localStroage` 的一些 API ：
```javascript
// 最常用的为上面三个
localStorage.setItem() //设置 localStorage
localStorage.getItem() //读取 localStorage
localStorage.removeItem() //移除 localStorage

localStorage.clear() //清空 localStorage
localStorage.key() //获取某个位置的键名
```
不过需要注意的是，`localStorage` 不能直接存储对象，需要先将对象转成 json 才能存储。否则你将看到这样的结果：

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2018-05-10-144803.png)

接下来我们来实际操作一下：
```html
  <div>
    <h4>设置</h4>
    <label>
      键：<input type="text" id="key">
    </label>
    <label>
      值：<input type="text" id="input">
    </label>
    <button type="button" onclick="setValue()">写入</button>
  </div>
  <div>
    <h4>读取</h4>
    <label>
      键：<input type="text" id="getKey">
    </label>
    <button type="button" onclick="getValue()">读取</button>
    <p>
      读取内容：<span id="value"></span>
    </p>
  </div>
  <div>
    <h4>清除</h4>
    <label>
      键：<input type="text" id="removeKey">
    </label>
    <button type="button" onclick="removeValue()">清除</button>
  </div>
```
```javascript
  // 设置
  function setValue(){
    var key = document.getElementById('key').value
    var value = document.getElementById('input').value
    localStorage.setItem(key,value)
  }
  // 获取
  function getValue(){
    var getKey = document.getElementById('getKey').value
    var value = localStorage.getItem(getKey)
    document.getElementById('value').innerText = value
  }
  // 移除
  function removeValue(){
    var removeKey = document.getElementById('removeKey').value
    localStorage.removeItem(removeKey)
  }
```

上面的代码渲染出来的界面应该这这样的：

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2018-05-10-144820.png)

然后，在对应输入框中输入内容就可以进行操作了。
至于存储的内容，我们可以在控制台的 Application 中看到：

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2018-05-10-144835.png)

值得注意的是，如果键名相同，之前的键值会被覆盖。并且，因为这些存储的内容可以在控制台被浏览和修改，所以这里并不推荐用来存一些比较私密的信息，避免信息的泄露。