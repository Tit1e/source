---
title: jsTree 不同节点不同右键菜单
date: 2018-07-23 20:36:59
tags: ['学习']
---

公司系统中有个菜单管理的模块，用到了 [jsTree](https://www.jstree.com/) 的右键菜单功能，但是有个问题，就是菜单只有两级，一级菜单允许添加子菜单，二级菜单不允许添加子菜单，这就需要根据选中的菜单渲染不同的右键菜单。网上找了一番，在 [这里](https://blog.csdn.net/m0_37355951/article/details/78320254) 找到了解决方法。我在自己博客再记录一次，为了加深印象，也为了日后方便查找。
<!-- more -->
### 引入资源
```html
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/jstree/3.2.1/themes/default/style.min.css" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/1.12.1/jquery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jstree/3.2.1/jstree.min.js"></script>
```

### 用于初始化的 DOM 节点
```html
<div id="jstree_demo_div"></div>
```

### 初始化
```js
//模拟数据
var DATA = [{
  "id": "276",
  "text": "分析",
  "parent_id": "0",
  "children": [{
    "parent_id": "276",
    "id": "281",
    "text": "监测概况"
  }]
}]
//初始化
$(function () {
  $('#jstree_demo_div').jstree({
    core: {
      'data': DATA, //数据
    },
    'contextmenu': {
      'items': customMenu //右键点击触发的函数，下面会讲到
    },
    'plugins': ['contextmenu'] //右键菜单插件，必须加载这个插件才能使用右键菜单
  })
})
```

### 不同右键菜单的关键
这个函数是让 jsTree 有不同右键菜单的关键，当右键点击的时候，会执行这个函数，我们只要根据某个参数来判断当前是什么菜单，然后根据这个菜单来 return 右键菜单的对象就行了。
```js
function customMenu(node) {
  var items = {
    "new": {
      "label": "新建子菜单",
      "action": function (data) {
        // code...
      }
    },
    "edit": {
      "label": "编辑菜单",
      "action": function (data) {
        // code...
      }
    },
    "delete": {
      "label": "删除菜单",
      "action": function (data) {
        // code...
      }
    }
  }
  // parent_id 为 0 的是一级菜单
  if(node.original.parent_id !== '0'){
    delete items.new
  }
  return items
}
```
[参考代码](https://github.com/Tit1e/Demos/tree/master/50-jsTree)

[demo](https://evolly.one/demos/50-jsTree/)