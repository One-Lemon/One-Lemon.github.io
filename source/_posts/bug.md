---
title: bug
date: 2019-05-29 11:26:22
author: one-lemon
tags:
	- bug
---

### 在 `router-link`标签上绑定事件

> 添加修饰符 `.native`

### 使用 `kepp-alive` 标签使用 `include`控制组件缓存

> 使用多组时候，使用逗号隔开，但是不能有空格

### 滚动条不同组件复用

> 

### JS 数组排序 a-z 

这里有一组字符串的数组

```JS
var arr = ["M","U","Z","H","B","K","S","N","T","C","E","J","I","T","I","R","P","R","C","C","S","H","I","C","P","M","D","H","B","N","G","B","A"];
```

对其进行排序

```JS
arr.sort(function(v1,v2){return v1>v2});
/["P", "A", "R", "M", "B", "K", "B", "G", "B", "C", "E", "J", "I", "H", "I", "D", "M", "H", "C", "C", "C", "H", "I", "N", "N", "P", "R", "S", "S", "T", "T", "U", "Z"]
```

这个结果是我们不想要的，但是按道理没错误鸭。

解决方法：

```JS
arr.sort(function(v1,v2){
    return v1>v2?1:-1;
})
```

#### vuex里面的sotre数据改变，但是没有触发getter并没触发，视图也不更新

**Vue 封装组件，然后传参，引用类型，出现了联动问题**

> 1. 传值时候使用 concat 让他成为一个新数组
> 2. 扩展运算符
> 3. return