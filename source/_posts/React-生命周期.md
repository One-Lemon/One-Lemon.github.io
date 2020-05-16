---
title: React 生命周期
date: 2018-09-23 15:08:23
tags:
	- React
	- 生命周期
---

# React 生命周期

> 在 React 中开始创建到销毁有着一系列的生命周期，对此，我表达一下自己对 React 生命周期的理解哈。

首先贴一张 React 官方的生命周期图

[React-生命周期](/medias/images/React-live.png)

## 创建

1. `constructor(props)` 构造函数

   - 在 React 创建时，调用一次 `constructor` 。
   - 参数 props，获取父组件传入的数据。

   - 能做些什么？
     1. `super(props)` 可以理解为修改当前 this 指向，如果你要在后面正确的使用当前的 this，那么就一定需要。
     2. 统一修改事件方法的 this 指向（事件方法修改 this 指向的方法之一），例：`this.onClick = this.onClick.bind(this)`。
     3. 初始化 state，例：`this.setState = { name: 'cxk' }`。

2. `static getDerivedStateFromProps(nextProps, nextState)` 静态方法 从 props 派生 state

   - props | state 变化前，对其进行操作。

   - 参数 `nextProps, nextState` 获取变化之后的数据。

   - 需要 `return` 返回值，返回 state，如果不需要修改 state 值，那么直接 `return null`。
   - 在此方法里面不能获取到 `this`。
   - 可以认为此方法替代了 `componentWillMount`。

3. `render()` 渲染 DOM

4. `componentDidMount(prevProps, prevState)` 组件挂载完成

   - 参数 `prevProps,prevState` 修改之前的参数。
   - 可以获得当前组件的 `this`，所以可以使用 `setState`，它将触发额外渲染，但此渲染会发生在浏览器更新屏幕之前。如此保证了即使在 `render()` 两次调用的情况下，用户也不会看到中间状态。
   - 这里适合发起网络请求去获取数据。

## 更新

1. `static getDerivedStateFromProps(nextProps, nextState)` 
2. `shouldComponentUpdate(nextProps, nextState)` 组件是否更新
   - 需要 `return` 返回值，返回波尔值，如果返回 `true` 则正常进行更新，返回 `false` 中断当前更新。
   - 能获取到 `this`。
3. `getSnapshotBeforeUpdate(prevProps, prevState)` 在组件更新前一刻
   - 能够获取 `this`。
   - 需要一个 `return` 返回值，这个数据将会在 `componentDidUpdate` 中以第三个参数 `snapshot` 所获取到。
4. `componentDidUpdate(prevProps, prevState, snapshot)` 组件更新完成
   -  能获取到 `this`。
   - 第三个参数 `snapshot` 是上一个生命周期函数的 `return` 值。

**注：在更新的生命周期函数里面要警惕使用 `this.setState`，如果使用一定要注意出口，否则会造成死循环。**

## 卸载

- `componentWillUnmont()` 组件卸载前
  - 通常都是在这里面清除监听之类，以免造成对其他组件的影响。