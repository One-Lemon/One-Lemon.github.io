---
title: Redux
date: 2018-09-24 10:45:36
tags:
	- React
	- Redux
---

1. 引入 redux `import redux from 'redux'`
2. 创建 strore 实例对象 `const store = redux.createStore((state, action) => { return xxx })` 这个函数默认进来一次，可以做为仓库的初始化工作。
3. 暴露 store `export default store`

- 获取仓库中的数据 `store.getState().xxx`

## 修改仓库的数据

`store.dispatch(action)` 派发一个动作

## 组件得到 store 改变之后修改自身数据

```javascript
store.subscribe()
```

Reducer 返回一个新的 state，真正修改的是 state

## action 是什么？

> action 就是一个包含 type 和 value 的对象

- `return Object.assign({}, state, { name: action.value })`
- `return { ...state, name: action.value }`
- `let newState = {...state};return newState`
- `let newState = JSON.parse(JSON.stringify(state))`

## action 创建函数

## reducer 拆分

## ANTD

## redux 中间件

### redux-ogger

### redyx-thunk

> 这个中间件的使用，让我们 `store.dispatch` 能支持接收到函数，以前只支持对象。
>
> 闭包
>
> dom 对象  ie 低版本 内存泄漏

### redux-promise

### redux-saga

## react-redux

### UI 组件与容器组件

#### UI 组件（展示组件）

> 一般来说 UI 组件，只负责 UI 的渲染，不负责数据的情况，它不需要 state 数据，它上面的数据全是容器组件来通过 props 传递下来的。所以一般我们可以将 UI 组件写成 function 组件。

#### 容器组件

> 它麦处理数据，负责跟 redux 打交道。

---

- `connect(mapStateToprops, mapDispatchToprops)(UI 组件)`
- Provider 组件

1. `yarn add react-redux`
2. 项目入口，使用 Provider 组件将 store 的数据 context 下去。
3. 实现 UI 组件，由 connect 帮我们去创建容器组件。

## react 路由

- react-router								Link
- react-router-dom（推荐） NavLink    高亮效果

### 提供的路由相关组件

- 路由组件（全局只能有一个）

  - BrowserRouter		history模式

  - HashRouter				hash模式

  表现形式不同	原理不同

- 单个的路由规则组件
  - Route
- 全局路由
  - Router
- 导航组件
  - Link
  - NavLink
- 只匹配一条规则
  - Switch	从上往下渲染，只匹配第一条
- Redirect
- widthRouter    方法    高阶组件

### 路由界面自动接收到的三个 prop，路由页面组件中的其他组件是获取不到的。

- history
  - `push()`
  - `replace()`
  - `go()`
  - `goBack()`
  - `goForward()`
- location
  - search  query 的参数
- match
  - params 动态路由