---
title: React基础
date: 2018-08-17 08:57:44
tags:
	- React
	- 基础
---

# React 

> React 是 Fackbook 当初不满意市面上的 MVC 框架而自主开发的一款 JS 库。严格来说 React 是 V 层。
>
> - 轻量级视图库
> - 虚拟DOM
> - 组件系统
> - 单项数据流
> - JSX语法
>
> 注：React 中没有指令系统

## React 使用

- CDN 引入
    1. React 核心内容
    
    2. React-dom 渲染真实 dom 的库
    3. babel 编译 JSX 语法
    
- 自己搭建 webpack

- 使用第三方或者官方的脚手架 `npx create-react-app <name>`
## JSX语法
1. 单个根元素
2. 单标签一定要闭合
3. img 标签的 alt 属性一定要有
4. 标签是小写字母，组件首字母大写
5. class -> className
6. label 标签不能使用for ，换成 htmlFor
7. 注释：使用 JS 的注释，并且使用 { } ，例如 `{ /* <P> </P>*/ } `
8. style 需要写在对象当中
## JSX 的差值表达式
如何在 JSX 中使用变量？

> 通过 { 变量 } 的差值表达式来使用变量。

- 差值表达式中能使简单的单个表达式，不能是语句也不能是条件或者循环语句，可以使用三目运算符。
- 如果想渲染字符串 `str = '<h1>qqq</h1>'` ，需要在标签添加属性 `dangerouslySetInnerHTML` 接受一个对象的 `value{__html: str}`。
- 如果要传递参数到标签属性使用 `title = {msg}`。
- `{ null } | { undefined } | { ‘’ } | { false }`没有内容，不会在页面上渲染任何东西。

## 组件

### 组件的定义

> react 没有全局组件的概念，使用哪个组件引入使用
> 组件的函数名直接拿去当标签名
React 中组件一共分两种：

1. 函数式组件
2. 类组件
#### 函数组件的定义：

1. 定义一个函数，然后 return 出来一段 JSX 语法。
2. 函数名字就是组件的名字，首字母大写。
#### 类定义的组件：

1. 定义一个类，类名就是组件名 首字母大写。
2. 这个类需要继承于 `React.Component` 或者 `React.PureComponent`  基础组件。
3. 类中的 render 函数是必须的，render函数的 return 出来的是一段 JSX 语法。

注：组件的模板内容，如果需要换行去写的话，那么请使用（）包裹起来

没有使用 React，为什么还要去引入?

> JSX 语法是一个语法糖，经过 babel 转换，由于转换出来的代码中使用了 React，所以 React 必须要引入。

```react
// JSX 语法糖
React.createElement('div', '参数', 'content');
```



## 组件的 props 属性
- props 是个集合， prop 是具体的某一个
- Vue 中使用 props 首先需要在组件中定义 props 的选项，而 React 中不需要。
- 函数组件中所有的 prop 会作为参数传递过来 
- 类组件所有的 prop 会在 `this.props` 身上

### prop-types 校验
1. 安装 prop-types 的模板 `npm i --save-dev prop-types`
2. 引入 prop-types
3. 设置组件的 propTypes 属性

---

React 元素 | 虚拟 DOM 元素 | JSX 代码中任意标签 | 通过 React.createElement 创建出来的 JS 对象

> 最基本的单元，任何的标签都可以看成是一个 React 元素
> 如何区分 React 元素 与 React 组件的概念
> 组件就是一系列 React 元素的组成
> 什么是元素变量
> 定义一个变量，变量的值是 React 元素
> React 元素 不是一个可变的对象
## state

数据绑定的方式有两种
1. props
2. state    私有属性
### 有状态组件 & 无状态组件
> 组件有没有状态，主要看这个组件有没有 state，一般类组件就可以称为有状态组件，函数组件叫无状态组件
### 实现自动跟新，需要让组件有state，当 state 发生变化，他就会重新渲染
- 组件变化方式
  1. 组件接收到的 props 发生改变
  2. 组件自身的 state 有了变化

defaultValue / defaultChecked默认值  ，单独使用 Value 会报错需要配合onChange使用
## 生命周期
挂载

- constructor 构造函数 组件实例化的时候触发，不能在里面 setState。
  1. 调用父类的构造函数 
  2. super(props) 初始化数据 
  3. 绑定this指向
- componentWillMount()  × 即将过期
- static getDerivedStateFromProps()  不常用，能够根据 props 的数据来设置新的 state 数据。
  1. 初始化 render 之前调用一次
  2. 后续数据有变化，重新 render 之前又会调用
  3. 不要使用 `this.setState` -> 哪些不能用？
  4. 需要有 return { } | null ，如果是对象，会将 state 合并
- render () 渲染  默认进来一次，后续如果有更新会再次触发 使用  setState 需要谨慎。
- componentDidMount() 组件挂载完成。 

  1. 获取异步数据
  2. 操作 dom


更新

- static getDerivedStateFromProps()
- shouldComponentUpdate(nextProps, nextState) 性能优化，这个组件是否需要进行更新渲染？推荐使用 PureComponent 组件继承，两者不能同时存在。需要返回值，返回 true -> 更新生命周期继续执行。false -> 生命周期不执行。
- render
- getSnapshotBeforeUpdate(prevProps, prevState)   真实 DOM 渲染完成的前一刻
- componentDidUpdate(*prevProps*, *prevState*, *snapshot*)  更新完成，对 DOM 进行操作，发送网络请求，但是需要正确进行 if 判断。

卸载

- componentWillUnmount  销毁

## 组件之间的通信

### 父 -> 子

- 通过props

### 子 -> 父

- 传递方法给子组件，然后子组件调用传递参数

### 兄弟组件

- 状态提升

- 第三方中央事件管理器(pubsub)

- context

  1. `let MyContext = React.createContext()` 创建出一个 context
  2. `MyContext.Prvider` 供应商组件
  3. `MyContext.Consumer` 消费者组件  标签中间需要使用函数返回 JSX，这个函数接受一个参数，这个参数就是供应商中的值。

  ---

  1. 使用上面的步骤
  2. 不使用消费者组件，弄成 `x.contextType = MyContext`。使用 `this.context` 获取供应商的数据。

- React 状态管理器

  - 官方 flux
  
  - 第三方  Redux
    1. 任何时刻 state 都没有被改变。
    
    2. 生成一个新的 state 去替换旧的 state，Reducers 不能修改，只能通过 return 纯函数。
    
       Tips：纯函数--任何时候都不会修改参数，有相同的入参，一定会产生相同的出参。
  - 第三方  

Tips：children 类似于 Vue 的插槽

