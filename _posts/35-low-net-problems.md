---
title: vue-cli 使用记录以及使用过程中遇到的问题总结
date: 2017-10-19 19:19:51
tags: ['学习', 'vue']
---
前段时间抽空重写了陋网,还没开发完成, 还有很多细节需要完善. 所以这篇博客随着开发还会更新.
<!-- more -->
### 配置记录
#### 注: 下面的文件都是以实际项目中的 `program` 目录为根目录来写路径的
#### `/package.json`
用过 `node.js` 的基本都知道, 主要用来管理包依赖,当然里面还会有些项目作者, 项目描述等配置,不过这都不是主要的.
说到依赖, 在 `/package.json` 中有两种依赖,分别是 `dependencies` 和 `devDependencies`, 从字面上大致可以看出两者的区别在于一个是生产环境用到的依赖, 一个是开发环境用到的依赖, 比如 `axios` 要在线上用于 `http` 请求, 所以这个依赖肯定得配置在 `dependencies` 下, 在安装的时候只要在命令后面加上 `--save` 就能自动写进 `dependencies`. 而像 `eslint` 这种依赖是在开发阶段用于代码检查的, 生产环境中不需要的,那么就配置在 `devDependencies` 下, 安装的时候在命令后面加上 `--save-dev`就自动写进 `devDependencies` 中.
#### `/config/index.js`
可配置 `index.html` 和 `static` 文件夹的打包输出路径.
#### `/build/webpack.base.conf`
可在这个文件下的 'resolve' 下的 `alias` 对象中配置资源引用路径的缩写. 	`vue-cli` 在构建的时候默认会有 `'@': resolve('src')` 这条配置, 意思是引用文件的时候, `src` 这个路径可以用 `@` 表示, 当路径长了这个配置的优势就会提现出来. 
### 项目记录
#### `/index.html`
入口文件, 项目最后打包的时候, 所有代码都会压缩到这个 `html` 文件中.
需要注意的是, 由于vue作了限制, 在文件中引用静态资源时, 静态资源不可放于src目录下, 要放于与这个文件同级的 `static` 目录下.
#### `sass`
要使用 `sass` 需要安装 `node-sass` 和 `sass-loader` 两个包
```
npm install node-sass --save-dev
npm install sass-loader --save-dev
```
然后在 `style` 标签中声明 `<style lang="scss"></style>` 就可以使用了.
#### `fastclick`
移动端用 `fastclick` 来解决300毫秒点击延迟问题.
`npm install fastclick --save`
在 `main.js` 中 import: `import fastclick from 'fastclick'` .
并且在绑定在 `body` 上: `fastclick.attach(document.body)` .
####  `better-scroll`
使用 `better-scroll` 来获得更好的移动端体验
`npm install better-scroll --save`
由于项目中需要滚动的地方都需要用到 `better-scroll`, 所以将它封装成一个通用的基础组件,需要使用的时候直接引用就可以了.
很多刚使用 `better-scroll` 的人会遇到使用了插件,并且初始化也成功了,但是却滚动不了. 网上一查有很多的相关问题,我在使用的时候也遇到了这个问题,其实在 `better-scroll` 的 github 上已经说得比较清楚了:
<img src="https://camo.githubusercontent.com/fd2fd41d5bc70502acb590d53f2084248587cb85/687474703a2f2f7374617469632e67616c696c656f2e7869616f6a756b656a692e636f6d2f7374617469632f746d732f736869656c642f7363726f6c6c2d342e706e67">
包裹的元素需要设置一个固定高度,当子元素的高度大于父元素的高度的时候就好出现滚动条. `better-scroll` 的核心原理就是禁用默认滚动条, 父元素设固定高度, 内容超出部分隐藏, 然后用 `css` 动画来做滚动效果.
所以只需要给父元素加个固定高度就可以解决页面不滚动问题.
#### `vue-router`
* 添加路由
在 `/src/router/index.js` 中可配置路由. 先将组件 import 进来, 然后在 `routes` 数组中添加路由配置. 一般路由格式如下:
```
{
  path: '/home',
  component: Home
}
```
* 默认路由
`redirect` 可配置项目的默认路由:
```
{
  path: '/',
  redirect: '/index'
}
```
* 嵌套路由
```
{
  path: '/home',
  children: [
    {
      path: '/home/index',
      component: Index
    }
  ]
}
```
* 动态路由
```
{
  path: '/people/id/:id',
  component: People
}
```
例子中的配置在地址中的渲染效果会是 `/people/id/1`.



未完待续...