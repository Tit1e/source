---
title: vuex的简单使用教程
date: 2017-12-26 21:11:39
tags: ['vue','学习']
---
公司数据平台现在的规模已经有点大了，考虑到以后，可能会出现数据共享的问题，所以我先粗略了解一下`vuex`以备不时之需。
首先安装`vuex`
```bash
#生产环境中也需要使用
npm install vuex --save
```
<!--more-->
`src`目录下新建`vuex`目录

vuex文件夹下新建`store.js`
先在`store.js`中写入
```javascript
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)
//state用于存储数据
const state = {
  count: 10
}
//mutations用于声明方法
const mutations = {
  add(state,n) {
    state.count += n
  },
  reduce(state,n) {
    state.count -= n
  }
}
//getters用于监听，比如数据变化时执行某个函数
const getters = {
    //count变化时就会执行
    count: state => {
        return console.log(state.count)
    }
}
//actions类似于mutations，但actions可以异步执行
const actions = {
  addAction({ commit }) {
    commit('add',1)
  },
  reduceAction({ commit }) {
    commit('reduce')
  }
}
//暴露这些对象
export default new Vuex.Store({
  state,
  mutations,
  getters,
  actions
})
```

组件中使用
```html
<tempate>
    <div>
        {{ count }}
        <button @click="add(10)">-</button>
        <button @click="reduce(10)">+</button>
    </div>
</template>
```
```javascript
//引入
import store from '../vuex/store'
//使用这种方式引入后，可以像平时一样使用store从的值与方法，其实还有另外两种调用方法，但我个人喜欢这种，这也是最简单的写法，所以另外两种我就不赘述了。
import { mapState,mapMutations,mapGetters,mapActions } from 'vuex'
export default {
    store,
    computed: {
        ...mapState(['count']),
        ...mapGetters(['count'])
      },
      methods:{
        ...mapMutations(['add','reduce']),
        ...mapActions(['addAction','reduceAction'])
      }
}
```