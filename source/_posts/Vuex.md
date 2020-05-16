---
title: Vuex
date: 2018-07-06 09:01:57
tags:
	- Vue
---

# Vuex 

> Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**。
>
> 注意：他是一个状态管路模式，切记不要当做数据库使用
## 什么时候用vuex

> 用于组件通信，通常在使用了路由之后，不同组件之间的通信， 或者组件之间通信很复杂的时候，这个时候 Vuex 就能帮你进行这个操作。

## Vuex的操作流程

1. 仓库，项目中组件上需要共享的数据放到仓库 state。
2. 组件要使用仓库中state的数据，就从仓库里面
3. 修改仓库中的state
    1. commit  mutation
    2. 派发 action -> commit mutation
4. 仓库中state数据发生变化，组件就会得到更新

## Vuex的使用

1. 安装vuex npm i --save vuex
2. src/store/index.js配置实例对象
3. main.js vue实例中配置store选项 选项的值就是 2 中的实例对象

## Vue-router 实例的对象

- state：数据存放

- mutations：修改 state 数据的方法

  ```JS
  mutations: {
      setOne (state, params){	//第一个参数为本地state，第二个为传入的参数，传入多个数据使用数组或者对象
          state.num = params
      }
  }
  ```

- modules：模块

- getters：类似于计算属性，参数为 state

- actions：异步操作，传入参数需要使用结构

  ```JS
   actions: {
      getCityList ({ state, commit }) {	//comit为mutation方法执行
        axios.get('地址').then(res => {
  		console.log(res)
        })
   	}
  ```

  

## 将仓库中的数据拿到组件中使用

- `this.$store` 就是仓库的实例对象  直接使用不推荐
- 通过计算属性 computed
-  vuex提供的辅助函数 `mapState('仓库名',['state']) `  key要和仓库中的相同

为了能够将 mapstate 和自身的 computed 结合 ，推荐使用下面这种方法：

```JS
import { mapActions, mapState, mapMutations } from 'vuex'
export default {
    computed: {
    ...mapState('demo', ['demoState'])
  },
  methods: {
    ...mapActions('demo', ['demoState']),
    ...mapMutations('demo', ['setCurFilmType']),
    fn () {
        console.log('自定义fn')
    }
}
```



## 修改仓库中的数据

1. 定义 `mutation` ，唯一一个可以修改 `state` 中数据的方法
2. 组件中提交 这个 `mutatiion`
    1. `this.$strore.commit('mutatiion名字'，参数)`
    2. `this.$store.commit({
    type: mutaion 名字
    其他参数
    })`
    3. 使用 `mapMutation` 辅助函数
### action

> `mutation` 不允许异步代码，当你在 `mutation` 中使用异步函数的时候会出现数据刷新不及时。
>
> 例如：你设置了一个点击发送请求，获取服务器传递进来的数据。当你点击了按钮，发送请求，并且可能在页面上也渲染出来了，但是你点击 `Vue` 工具就会发现数据没有改变，再次点击数据是上一次发送请求传回来的数据。有时候你需要对数据进行判断操作的时候，使用 `mutation` 发送请求就会出现错误。

### actions 写异步代码  
> 每一个action里面都可以写异步代码，但是不能修改state里面数据，修改数据的还是 `mutaction`

### 推荐在 vuex moduels 每个模块都加上命名空间

1. 操作方便
2. 便于模块分类