---
title: Vue-router
date: 2018-06-31 10:04:32
tags: 
	- Vue
summary: Vue-router-创建单页应用，是非常简单的，这里的路由就是SPA（单页应用）的路径管理器
---

# Vue-router

> 创建单页应用，是非常简单的，**这里的路由就是SPA（单页应用）的路径管理器。**

## Vue-router 的使用
1. 安装 `npm i vue-router`。

2. 在 `src` 目录下创建 `router` 文件夹，并创建 `index.js` 对其进行路由的配置。

   ```JS
   import Vue from 'vue'
   import VueRouter from 'vue-router'	//引入vue-router模块
   Vue.use(VueRouter)
   const router = new VueRouter({	//实例化路由对象
       routes: [
           {
               path: '/index',	//路径匹配
               component: () => import('../views/index.vue')	//懒加载，组件位置
           }
       ]
   })
   export default router	//暴露
   ```

   

3. 在 `main.js` 中引入并且加入 Vue 实例中。

   ```JS
   import Vue from 'vue'
   import App from './App.vue'
   import router from './router'
   
   new Vue({
       router,
       render: h => h(App)
   }).$mount('#app')
   ```

   

4. 使用组件 `<router-view></router-view>`，通过这个组件可以嵌套路由。

## router-link 和 router-view

> `router-link` 是个全局组件，类似于 a标签。to 属性：hash地址，类似于 a 标签的 `href`。
>
> `router-view` 路由嵌套。
## 动态路由匹配

> 使用了 `vue-router` 以后，在 vue 的实例中添加了 `$route` 和 `$router` 两个对象。
>
> `$route` ：当前匹配的路由对象信息，可以获得跳转来自、目的路径、参数等信息传递
> `$router` ： 路由器的实例对象，可以进行编程式导航。

### 编程式导航

>简单的来说，就是通过 JS 代码进行跳转。

### 使用 `$router` 对象调用方法进行导航

- push()	跳转页面 新增一个历史记录，该方法接受参数

  ```JS
  router.push('index')	//router中配置的name
  router.push({ path: 'index' })
  router.push({ name: 'index', params: { userId: '123' }})
  router.push({ path: 'index', query: { userId: '123' }})	// 带查询参数，变成 /index?userId=123
  ```

- back()	后退

- forward()	前进

- go()	根据参数前进还是后退，正数前进，负数后退

- replace()	跳转页面 重定向页面 不加历史记录，使用方法和 push 方法相同，通常使用在登录之类

**注：name 和 params 搭配 相当于 POST 请求，参数需要通过 $route 获取，name 不能和path使用，path和query搭配 相当于 GET 请求。**

# 路由的两种模式

> `router` 下的 `index.js` 中的 `router` 实例中添加 `model: 'history'`

- hash 默认模式

  使用 URL 的 hash 来模拟完整的 URL ，所以当 URL 发生改变的时候，页面不会重新加载，也不会 404 报错，就是外观上略丑。

- histroy 模式
利用了 HTML5 History Interface 中新增的 `pushState()` 和 `replaceState()` 方法，模拟出真实的 URL 路径，但是一刷新就会 404 报错，因为重新请求这个 URL 对应的地址是不存在的，所以需要后台配置相关的路由处理。
## 区别
1. hash模式会在 URL 地址上会有一个 # 号，histroy 没有。
2. 原理上，hash通过 `window.onHashChage`  这个事件来处理，histroy 基于 HTML5 中 histroy 新增的一些 `api. histroy.pushState(),api.histroy.replaceState(),window.onpoopstat 。
3. histroy 需要后台配合处理上线的 404 问题。
# 导航守卫

> 当我们路由发生改变的时候，可以通过导航守卫做我们想做的。

- 全局守卫
   - 全局前置  `beforeEach()` 接收三个参数
       to	要去的路由
       from	来自哪个路由
       next	是否执行 'to' ，直接调用那就相当于放行，如果传递一个false，那么就不放行，不调用不放行，调用并且里面可以传递路由的path路径或者是路由的对象信息，那么就可以重定向我们的参数中所指定的位置。

   - 全局解析守卫 `beforeResolve`
   -   全局后置   `afterEach()`
       没有 `next` 参数
- 路由独享
       `beforeRouteEnter`：进入当前组件，**不能访问this，因为此时组件还没被创建**
       `beforeRouteUpdate`：当前组件更新
       `beforeRouteLeave`：离开当前组件
- 组件级别
### 导航守卫的钩子函数

> 路由发生变化时候主动触发的一些函数

### 作用场景

1. `beforeEach,afterEach` 实现页面进度条
2. 登录拦截