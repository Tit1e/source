---
title: Javascript循环添加
date: 2017-02-02 21:23:03
tags: ['学习']
---
工作中遇到的一种挺好的循环添加列表的方式，用模板，记录一下。

以前遇到要循环添加相同的元素用的都是字符串拼接的方式，后来工作中遇到了这种方式，发现这个比字符串拼接好用很多，简洁明了。

### HTML

``` HTML
<!--目标div-->
<div id="div">
</div>
<!--模板div,模板所在div不会被复制-->
<div id="test" style="display: none;">
    <h1>hello,my name is TITLE</h1>
    <p>I'm CONTENT years old!</p>
</div>
```
<!-- more -->
### Javascript

``` javascript
//模拟后端传过来的json数据
var data = {
    datali: [{
        name: "chkl1",
        age: "sad1"
    }, {
        name: "chkl2",
        age: "sad2"
    }, {
        name: "chkl3",
        age: "sad3"
    }, {
        name: "chkl4",
        age: "sad4"
    }, {
        name: "chkl5",
        age: "sad5"
    }]
}
var lilength = data.datali.length;
var arr = data.datali;
for (var i = 0; i < arr.length; i++) {
    <!--获取模板div中的html-->
    var tihuan = document.getElementById("test").innerHTML;
    <!--用replace替换模板中要替换的部分，替换顺序最好是先替换长的再替换短的-->
    tihuan = tihuan.replace(/TITLE/, arr[i].name)
        .replace(/CONTENT/, arr[i].age);
    <!--创建一个div将修改完成后的代码放进去，再将这个div添加到目标位置-->
    div = document.createElement("div");
    div.innerHTML = tihuan;
    document.getElementById("div").appendChild(div);
}
```
注：模板中只能用class不能使用id。