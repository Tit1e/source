---
title: 使用 Vue CLI3 编写组件库并发布至 npm
date: 2019-03-26 00:05:29
tags: ['vue', '学习']
---

前几天研发部老大来跟我说，由于现在公司业务中台化，基本上公司所有的系统都要进行迁移，并将原来使用的jQuery + Bootstrap 技术栈更新为 Vue + Element。既然统一了技术栈，那么在开发过程中肯定会有很多组件是可以共用的，甚至登录页。因此要求我开发一个公共组件库，以避免开发过程中写重复代码，提高开发效率。我也正好趁此机会了解一下相关的知识。

先放上组件地址：[hzsj-components](https://www.npmjs.com/package/hzsj-components)。因为不涉及公司业务，所以就把它发布到了 npm 上。

这个组件库是在  [Element](<http://element-cn.eleme.io/#/zh-CN>) 的基础上进行开发的，而且是第一次开发，不了解如何合理配置目录，因此我的组件库目录部分参考了 [element-ui](<https://github.com/ElemeFE/element>) 的结构。

<!-- more -->

以  `HzCopy` 组件为例：

所有组件都放置在 `/packages` 目录下，`/packages/index.js` 用于整合所有组件，导出整个组件库。而 `/packages/copy/index.js` 用于导出单个组件，比如需要按需引入组件时。

```javascript
// /packages/index.js
import Copy from './copy'

// 存储组件列表
const components = [
  Copy
]

// 定义 install 方法，接收 Vue 作为参数。如果使用 use 注册插件，则所有的组件都将被注册
const install = function (Vue) {
  // 判断是否安装
  if (install.installed) return
  // 遍历注册全局组件
  components.map(component => Vue.component(component.name, component))
}

// 判断是否是直接引入文件
if (typeof window !== 'undefined' && window.Vue) {
  install(window.Vue)
}

export default {
  install,
  Copy
}
```

```javascript
// /packages/copy/index.js
// 导入组件，组件必须声明 name
import HzCopy from './src'

// 为组件提供 install 安装方法，供按需引入
HzCopy.install = function(Vue) {
  Vue.component(HzCopy.name, HzCopy)
}

// 默认导出组件
export default HzCopy
```

这样就编写完了一个组件，但要发布还得做一些其他工作。

完善 `package.json`：

* name：包名，就是  `npm install`  的时候输入的包名

* version：版本

* description：对包的描述，有利于用户搜索

* keywords：关键字，以空格分离，有利于搜索

* private：是否私有，必须修改为 false

* author：作者

* license：开源协议

* repository：在 npm 介绍页面显示项目入口，参考格式

  ```json
  "repository": {
    "type": "git",
    "url": "https://github.com/xxxx/xxxx.git"
  }
  ```

* scripts：新增一条打包库的命令

  * `--target`： 构建目标，默认为应用模式。这里修改为 `lib` 启用库模式。
  * `--dest`：输出目录，默认 `dist`。这里我们改成 `lib`。
  * `[entry]`： 最后一个参数为入口文件，默认为  `src/App.vue`。这里我们指定编译  `packages/`  组件库目录。

  ```json
  "scripts": {
    "lib": "vue-cli-service build --target lib --name hzsj-components --dest lib packages/index.js"
  }
  ```

  

* main：定义包的入口文件

  ```json
  "main": "lib/hzsj-components.umd.min.js"
  ```

  

* engines：告诉用户运行你的包对 NodeJs 版本的要求

  ```json
  "engines": {
    "node": "v11.8.0"
  }
  ```

  

修改好  `package.json` 后就可以准备发布包了。

首先如果使用了淘宝或其他镜像需先设置回 npm 镜像

```bash
npm config set registry http://registry.npmjs.org 
```

或者如果使用 [nrm](<https://www.npmjs.com/package/nrm>) 可使用：

```bash
nrm use npm
```

然后去 [npm](<https://www.npmjs.com/>) 注册一个账号，注册完了在终端登录

```bash
npm login
```

发布

```bash
npm publish
```

如果你的包名以 @ 开头，用上面命令可能会发布失败，需要用以下命令发布

```bash
npm publish --access public
```

[参考文章](<https://juejin.im/post/5bbab9de5188255c8c0cb0e3>)

[参考文章2](<https://juejin.im/post/5b231f6ff265da595f0d2540>)

[参考文章3](<https://blog.csdn.net/u013727805/article/details/80849329>)

