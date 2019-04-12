---
title: 一些工作中常用到的代码(二)
date: 2017-07-22 20:11:41
tags: ['学习笔记']
---
## jQuery操作单选、复选框选中状态
工作中时常会遇到使用 jQuery 操作选中、取消选中状态，之前是通过添加和移除 `checked` 属性来操作的，但是这种方法会在判断选中状态时会出现不一致的问题。使用如下方法就不会有什么问题(与第一次的内容有点重复)。
```javascript
    //修改选中状态
    $('input[type=checkbox],input[type=radio]').prop('checked',true);//选中
    $('input[type=checkbox],input[type=radio]').prop('checked',false);//取消选中
    //判断是否选中
    $('input[type=checkbox],input[type=radio]').prop('checked');
```

<!-- more -->
## 使用FormData对象异步传输 form 表单数据
我们工作中有时候会遇到表单需要用 Ajax 的方式传输，但是如果遇到表单元素数量比较多的情况，获取表单的数据内容会占据大量的时间，而 `formData` 可以将表单中的数据像提交表单那样将表单中的数据“打包”，然后通过 Ajax 传输。
```html
    <form id="uploadForm" enctype="multipart/form-data">
        <!-- code... -->
    </form>
    <script>
        $.ajax({
            url: '/upload',
            type: 'POST',
            cache: false,
            data: new FormData($('#uploadForm')[0]),
            processData: false,
            contentType: false
        }).done(function(res) {

        }).fail(function(res) {

        });
    </script>
```
**注意点**
* `processData` 设置为 `false`。因为 `data` 值是 `FormData` 对象，不需要对数据做处理。
* `form` 标签添加 `enctype="multipart/form-data"` 属性。
* `cache` 设置为 `false`，上传文件不需要缓存。
* `contentType` 设置为`false`。因为是由 `form` 表单构造的 `FormData`对象，且已经声明了属性 `enctype="multipart/form-data"`，所以这里设置为 `false`。
## 使网页文字无法被选中
给 html 标签加上 `onselectstart="return false"` 就可以实现
```html
    <body onselectstart="return false">

    </body>
```
## 不让 input 输入框显示历史输入记录
```html
    <input type="text" autocomplete="off" />
```
## layDate日期插件，样式发生错位
layDate日期插件在与 bootstrap 一起使用的时候，layDate的日期选择框按钮会发生错位的现象，在layDate的 css 文件中加入一下几行样式可解决：
```css
    .laydate_box, .laydate_box * {
        box-sizing:content-box;
    }
```
## 代码触发 change 事件
有些时候，我们希望用代码触发change事件，可以直接调用无参数的 `change()`方法来触发该事件。