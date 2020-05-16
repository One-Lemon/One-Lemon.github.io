---
title: vue 基础知识2
date: 2018-06-28 14:09:04
author: one-lemn
summary: Vue 的基本知识了解
tags:
	- Vue
	- 基础
	- JS
---

# Vue

## Class 与 Style 绑定

> 当有时候我们需要绑定 class 或者是动态切换 class 的时候，我们可以通过 `v-bind:class` 来实现。

### class 绑定

语法：`:class='{ 类名: 条件 }'`  -- 这里的条件可以是表达式，也可以是 data 的数据，但是结果需要是 booleran。可以使用多组判断条件，使其能够实现不同条件有不同的样式。

```html
	<div id="box" >
        <div :class="{ 'box': isHave , 'box1': isOk}"> 这是一个bbbox</div>
        <div> 这是一个bbbox33</div>
    </div>
    <script>
        new Vue({
            el: '#box',
            data: {
                isHave: false,
                isOk: true
            }
        })
    </script>
```

### 绑定到内联样式

直接将 `：style` 绑定一个对象，里面是 css 样式，这是最简洁明了的。

## 事件

### 事件修饰符

在监听事件的时候，我们可能需要一些特殊的处理，比如去掉默认行为，或者是禁止事件冒泡，vue 为我们提供了很简便的方法，指令后面添加事件修饰符。

```html
<div id='box' @click.prevent='fn'> </div>
```

- `.stop`	阻止事件冒泡
- `.prevent`	阻止默认行为
- `.capture`	事件捕获
- `.self`	     事件由自身触发  参考 `onmouseout` 和 `onmouseleave`
- `.once`	单次事件
- `.passive`	默认行为，这个就有点特别了，因为如果每次事件产生，浏览器都会去查询是否有 `preventDefault` 阻止该次事件的默认动作，使用它是为了告诉浏览器，不用查询了，我告诉你没有阻止默认动作。通常用在高频率事件中，比如滚动监听、鼠标移动，因为 1px 都会有事件产生，这样就能减少大量的资源。

### 按键修饰符

> 例：`@keyup.enter='submit'`
>
> 当你按回车时候提交表单
>
> 注：`Vue.config.keyCodes.abc = 222`	自定义按键

同理还有鼠标按键修饰符，系统修饰键，异曲同工。



