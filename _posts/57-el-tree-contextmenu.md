---
title: element-ui 树形控件使用右键菜单
date: 2019-03-29 21:16:20
tags: ['vue','学习']
---

element-ui 的树形控件本身是不支持右键的，官方文档上推荐把操作按钮展示在对应的项上，但是如果操作项比较多，或者本身就有信息需要展示在项上，再加上这些操作按钮就会比较凌乱。正好公司业务上有这个需求，因此我在网上搜了一圈，找到比较好的解决方式。

整体思路就是：当右键时，调用左键的 `node-click` 方法，通过 `node-click` 将获取到的数据储存起来，然后展示右键菜单，获取到数据后，怎么处理就可以自由发挥了。

右键菜单我使用了 [vue-context-menu](<https://github.com/xunleif2e/vue-context-menu>) 插件，引入方式可看 `README`。

[Demo](https://evolly.one/demos/57-el-tree-contextmenu/)

下面是具体代码：

```html
<div id="app">
  <el-tree :data="data" id="el-tree" :props="defaultProps" @node-click="handleNodeClick"></el-tree>
  <context-menu class="right-menu"
      :target="contextMenuTarget"
      :show="contextMenuVisible"
      @update:show="(show) => contextMenuVisible = show">
    <a href="javascript:;" @click="handleClick">操作1</a>
    <a href="javascript:;" @click="handleClick">操作2</a>
    <a href="javascript:;" @click="handleClick">操作3</a>
  </context-menu>
  当前右键获取到的节点为：{{JSON.stringify(nodeData)}}
</div>
```

```javascript
export default {
  data() {
    return {
      contextMenuTarget: null,
      contextMenuVisible: false,
      nodeData: {},
      data: [{
        label: '一级 1',
        children: [{
          label: '二级 1-1',
          children: [{
            label: '三级 1-1-1'
          }]
        }]
      }, {
        label: '一级 2',
        children: [{
          label: '二级 2-1',
          children: [{
            label: '三级 2-1-1'
          }]
        }, {
          label: '二级 2-2',
          children: [{
            label: '三级 2-2-1'
          }]
        }]
      }, {
        label: '一级 3',
        children: [{
          label: '二级 3-1',
          children: [{
            label: '三级 3-1-1'
          }]
        }, {
          label: '二级 3-2',
          children: [{
            label: '三级 3-2-1'
          }]
        }]
      }],
      defaultProps: {
        children: 'children',
        label: 'label'
      }
    }
  },
  mounted() {
    this.$nextTick(() => {
      // vue-context-menu 需要传入一个触发右键事件的元素，等页面 dom 渲染完毕后才可获取
      this.contextMenuTarget = document.querySelector('#el-tree')
      // 获取所有的 treeitem，循环监听右键事件
      const tree = document.querySelectorAll('#el-tree [role="treeitem"]')
      tree.forEach(i => {
        i.addEventListener('contextmenu',event => {
          // 如果右键了，则模拟点击这个treeitem
          event.target.click()
        })
      })
    })
  },
  methods: {
    handleNodeClick(data) {
      console.log(data)
      this.nodeData = data
    },
    handleClick(){
      this.contextMenuVisible = false
    }
  }
}
```

```css
html,
body {
  height: 100%;
  font-size: 14px;
}
#app {
  width: 800px;
  margin: 0 auto;
  font-family: 'Microsoft Yahei', 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  height: 100%;
}
// 右键会选中文字，为了美观将它禁用
#el-tree{
  user-select:none;
}
.right-menu {
  position: fixed;
  background: #fff;
  border: solid 1px rgba(0, 0, 0, .2);
  border-radius: 3px;
  z-index: 999;
  display: none;
}
.right-menu a {
  width: 75px;
  height: 28px;
  line-height: 28px;
  text-align: center;
  display: block;
  color: #1a1a1a;
}
.right-menu a:hover {
  background: #eee;
  color: #fff;
}
.right-menu {
    border: 1px solid #eee;
    box-shadow: 0 0.5em 1em 0 rgba(0,0,0,.1);
    border-radius: 1px;
}
a {
    text-decoration: none;
}
.right-menu a {
    padding: 2px;
}
.right-menu a:hover {
    background: #42b983;
}
```

以上就是完整代码。

当然还有另外一种思路。el-tree 自带有 `node-contextmenu` 事件，如果使用了这个事件，原生的右键事件都会被阻止，因此上面方法不能与这个事件一起使用，会造成右键插件无法正常显示。但是 `node-contextmenu` 事件返回了非常详细的参数：依次为 event、传递给 `data` 属性的数组中该节点所对应的对象、节点对应的 Node、节点组件本身。由于可以获取到 event，里面带有鼠标右击的坐标，因此完全可以自己写一个右键菜单，而且这个事件的回调参数更为丰富。

[参考文章](<https://segmentfault.com/q/1010000012940760>)