---
title: Vue 基础知识1
date: 2018-06-27 14:40:57
author: one-lemon
summary: vue 是什么？构建用户界面的渐进式框架 
tags:	
	- Vue
	- 基础	
	- JS
---

# Vue

> vue 是什么？
>
> 构建用户界面的渐进式框架	MVVM 
>
> 原理：ObjectdefinePropety 修改属性的get，set方法，作为 vue 的核心，是通过 Observer,Dep,Watcher,Complie 四个类以及 CpompileUtil
>
> [vue](https://cn.vuejs.org/v2/guide/installation.html) 下载地址

## Vue 基础语法 

### 基本使用步骤

1. 下载并且引入 Vue.js

2. 实例化一个对象  

   ```js
   new Vue({
       el: '#box',	//挂载点  你需要操作的节点
       data:{
           msg:'hello vue'
       },	//存放的数据
       methods:{	//绑定事件的函数存放处
           fn: function(){}
       }     
   })
   ```

3. 差值语法 `{{ msg }}`，可以是数据、 JS 表达式

### dom元素内添加属性

- 事件的绑定 `v-on:click = 'fn'` 简写： `@click`

- 属性的绑定 `v-bind:title = 'msg'` 简写： `:title`

  > 类似于 Jquery 的 attr 方法，例：你要绑定一个链接到百度，那么你可以使用 `:href = 'url'`，然后在 Vue的实例对象中的 data 中添加 `url:'http://www.baiducom'`

- 双向绑定 `v-model = 'msg'` \

  > 在挂载点中添加文本框，然后使用该绑定方法，那么 msg 的值会随着文本框的内容改变，反之，文本框的内容也会随着 msg 的值变化

- 单次变化，默认绑定一次，后续数据更新不会重新改变 `v-once`

- 解析 HTML ，使用 `v-html` => 尽量少用，防止 XSS 攻击

### 在 Vue 的实例中选项

- el 对应的是 dom 节点 ， 还可以使用 vue 的实例对象.$mout() 设置挂载点

- methods 绑定事件的方法存放处

- data 存放的数据

- computed  计算属性

- watch 监听器   当你在控制台改变 data 中 msg 的时候，会执行该方法，打印出两个值，起到监听作用

  ```js
  watch:{
  	msg: function(newValue,oldValue){	
          console.log(newValue);
          console.log(oldValue);
      }
  }
  ```

  

Tips：v-cloak vue 定义的属性，能解决页面闪烁跳动的效果 

> 在 css 样式中使用属性选择器来选定 v-clock 设置属性为 `display:none` 

### Vue常用指令  （通常以  v- 开头，用在元素的标签上面，借鉴 angluar）

- v-if 类似于 JS 当中的判断，如果对应的属性为真时显示内容，为假时改节点都不会在 F12 中存在，控制 dom 的存在与否

- v-show 和 v-if 也是能隐藏节点 但是他和 v-if 有不同的地方，首先 v-if 是通过删除和创建 dom 的方式，而 v-show 则是使用 `display:none` 控制消失和显示，控制 dom 的显示与否

- v-for 循环一组数据来渲染 dom 结构

  如果在 v-for 没有给循环的每一项设置一个唯一标识符，那么后续这个数据发生变化，页面重新渲染，浪费性能
  
  注：在组件中，v-for 必须要有 key

Tips：了解 v-bind:[attrname]  /  修饰符 . 

### Vue 数据的检测

- 数组：使用一些变异方法或者直接替换数据都能引起页面的更新。但是以下两个操作不会：

  1. 直接根据数据下标来修改
  2. 直接修改数据的长度

  解决方法：使用 Vue.set 原型方法或者使用 vm.$set 实例

  ```js
  Vue.set(target, index, value)；
  Vm.$set(target, index, value)；
  ```

- 对象：Vue 不能检测对象属性的添加或删除 ，添加解决方法同上 index 为 key 值

  ```js
  Vue.delete(target,key);//删除对象属性
  ```

---

## 小总结：

### 计算属性、方法、侦听属性都有什么区别？什么时候使用?

> 计算属性存放在 computed ，方法在 methods ，侦听属性在 watched。当你使用 methods 使用函数的时候，无论数据是否改变，都会执行这个函数，computed 只会在 msg 发生变化才会被执行。当你执行异步操作或者运算开销较大时候可以使用watch

### v-show 有着更高的首次渲染开销，v-if 有更高的切换开销。原因是什么呢？

> 因为 v-show 无论你是 true 还是 false 你都需要把 dom 结构给渲染出来，但是只需要渲染一次，因为之后的切换显示隐藏都是基于 css 属性 `display` 来执行的。
>
> 当你是 false 时候 v-if 执行，此时 dom 整个都不存在，渲染开销远大于 v-show，但是之后的切换每次都要重新添加 dom 进行渲染、删除等操作，十分消耗资源，所以频繁切换操作时候推荐使用 v-show

### 当使用条件渲染的时候，有时候会使页面出现布局变动，怎么解决呢？

> 通过将 vue 代码添加在 template 标签中，在浏览器中能够发现 条件渲染依旧能执行，但是这个标签并不会存留。正因为这个特点 template 不能和 v-show 搭配使用，因为这个标签不存留，所以不存在 `display` 的操作。

### 当你需循环判断某个对象的数据的时候，尽量不要使用 v-if 和 v-for 嵌套

> 因为这样做计算次数多，增加内容数据少的时候没什么区别，但是数据较为庞大的时候，就很明显了。这里推荐使用 `filter` 过滤，例：
>
> ```html
> <div id="box" v-for="(item, index) in items.filter(item => item.isOk)">
>     {{ item }}
> </div>
> ```
>
> Tips：箭头函数的函数体部分如果没有花括号，且只有一条语句会自动return

### v-text 、v-html 、差值表达式都有什么区别？

> 通常情况下，差值表达式的弊端是在网速较慢的情况下出现闪烁的问题，出现 `{{ msg }}` ，需要使用 `v-cloak` 可以解决这个问题。
>
> v-text 和 v-html 会把标签内部所有元素都给覆盖，并且 v-html 能解析 msg 中的 HTML 标签