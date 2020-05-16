---
title: Vue 组件
date: 2018-06-29 17:22:18
author: One-Lemon
top: true
cover: true
summary: 
tags:
	- Vue
	- 基础
	- JS
---

# Vue 组件

> 组件是 vue.js 最强大，也是最重要的功能之一。组件可以拓展 HTML 元素，并且对其进行封装重用的代码，提高代码的复用性。所有的 vue 组件同时也都是 vue 的实例。

## 怎么去注册组件？

### 注册组件

- 全局注册组件

  ```js
  Vue.component('组件名',{'选项对象'})
  ```

  组件对象是相当于 HTML 中的标签中使用，特别注意以下两点：

  1. 全局注册的组件需要在 Vue 所在挂载点的实例之前，否则会出现组件未注册的报错。
  2. 组件可以认为是 vue 的实例，但是选项对象中不能使用 `el` 和 `propsData`

- 局部注册组件

  ```js
  new Vue({
      el: '#app',
      components:{
          '组件名': {
              '选项对象'
          }
      }
  })
  ```

  局部注册的可以在同一个 vue 实例中创建多个组件

### 组件的选项对象

> 组件的选项对象中一定要有 `template` ，因为这是渲染到页面中的模板。组件可以认为是 vue 的实例，但是选项对象中不能使用 `el` 和 `propsData`

- `template`  

  组件的选项对象中需要用 `template:"<p>组件</P>"` 选项去定义模板，而且可以使用模板字符串，但是有时候这种方法总显得很复杂，所以有几种方法供我们使用：

  1. 直接使用 " " 或者 `` ，在模板字符串中你可以随意的换行，但是没有代码提示和高亮。

     ```js
     template: `<button> 这是组件 </button>`
     ```

     

  2. 利用 `template` 标签的特性，将组件里面 `template` 内容写在标签内，然后组件的 `template` 的值填 `template` 标签的 id 。`template: '#template-box'`

     Tips：`template` 标签不会被渲染出来，

     ```html
     <template id="template-box">
     	<div>
             <button>组件的按钮</button>
             <a>组件的a标签</a>
         </div>
     </template>
     ```

     
     
     注：`template` 标签内只能有一个根节点，如果你要使用多个标签，就需要用一个标签，如上面的 div 包裹住，否则会有 `Error compiling template` 报错，提示你只能有一个根节点。

- `data`

  在 vue 的实例中，`data` 选项可以直接使用，但是在组件中需要以函数的方式，并且使用· `return` 来返回数据对象。

  ```js
  Vue.component('my-tag',{
      data(){
          return {
                  msg: '这是组件的data中的msg'
              }
          },
  	template:'<p> {{ msg }} </p>'
  })
  ```

  

  因为组件通常是循环使用的，然后如果你组件还是直接使用 `data:{ msg: 'msg'}` 的方式的话，这里就有一个问题，这里的 `data` 是引用类型传参，你的多个 `my-tag` 组件都是指向同一个 `msg` ，当你只想修改其中一个的时候，这里就会造成全都被修改的问题，所以需要使用 `return` ，这样每次都是一个不同地址的数据对象。

- `props`

  > 这里涉及到了组件的嵌套，我们后面再提。现在需要知道的是父组件传数据给子组件时，子组件需要由 `props` 来获取，并且**`props` 中的数据不能被修改！**准确来说是能被修改，但是会报错，所以要修改数据，就在父组件的`data` 去修改。

其他的对象选项和 vue 实例中基本相同

## 组件的使用

> 通过标签的形式去使用，将组件名当做标签名在挂载点内使用，标签内任何内容都会被 `component` 选项覆盖。
>
> **注：如果组件名是 newBox 驼峰命名法，那么使用的时候标签名需要改成 `<new-box></new-box>`，标签也可以使用单标签哦，注意闭合。**
>
> props 的驼峰命名法也会有这个问题，因为在 HTML 会把大写字符解释为小写，如果使用字符串模板就不会有这个问题。所以在父组件传递参数给子组件时候需要在 HTML 内容中写例： ` <com-one :p-msg="msg"></com-one>`，但是在子组件的 `props` 中是 `props:['pMsg']`。

```html
<div id="app">
	<new-box></new-box>	//自定义组件
</div>
```



## 组件的嵌套

> 在某些时候，我们需要对组件进行一个嵌套，而 vue 的组件嵌套时很简单，只需要在父组件中添加子组件标签即可。
>
> 这个例子涉及到了组件通信的问题，不要慌，继续往下看。

```html
    <div id="app">
         <child :app-msg="msg"></child>
        {{ msg }}
    </div>
    <script>
		 new Vue({
            el: '#app',
           	data: {
                msg: '父组件的msg'
            },
            components: {
                child: {
                    data() {
                        return {
                            msg: '子组件的msg',
                        }
                    },
                    props: ['appMsg'],                   
                    template: '<p> {{ msg }} ----- 这是子组件的P标签----- {{ appMsg }} </p>'
                }
            }
    </script>
```



执行结果为：`子组件的msg ----- 这是子组件的P标签-----父组件的msg`

**注意：不要踩命名的坑，之前说到当你 `template` 不是模板字符串的方式的时候，你需要注意标签名是横杠连接写法（这里讲的俗点）。但是还需要注意的一个点就是在 `:app-msg='msg'` ，尽量注意，如果你使用驼峰命名法子组件使用时候接受的也是全小写！你使用的是 - 连接时候，使用的时候就需要是 驼峰式。我也是晕的很，这个还是自己要注意，并且多多实践。模板字符串一劳永逸呀。**

PS：自定义事件用`kebab-case`（- 链接）, 自定义属性用`camelCased`（驼峰式）

## 父子组件通信

> 有时候我们可能会碰到一些情况，就是子组件要获取或者修改父组件中的数据，又或者是父组件去访问子组件中的数据。但是不能直接去访问，需要通过一些方法来实现。

- `ref` 在元素上添加 `ref=' '`，然后可以在组件中使用方法 `this.$refs` 获取到与其相关联的组件。（不推荐）

### 父组件传递数据给子组件

1. 在父组件中使用子组件的时候自定义一个指令，用于接受父组件传递的数据。

   子模板中自定义指令 `app-msg` ，`msg` 是父组件 data 中的数据，需要对应名称！

   ```HTML
       <div id="app">	//父组件
           <child :app-msg="msg" ref="a"></child>	//子组件
       </div>
   ```

   

2. 子组件中通过 `props`选项，子组件获取到父组件传递进来的数据。

   ```HTML
    components: {
         child: {
     	  props: ['app-msg'],
     	 template: '<p ref="b">  ----- 这是子组件的P标签-----{{ appMsg }} </p>'
   	}
   }
   ```

   

### 子组件传递数据给父组件  

> 这个方法也可以用作子组件去调用父组件的方法。

1. 同样的，使用子组件的时候，自定义一个事件，父组件传递方法名给这个事件

   ```HTML
       <div id="app">
           <child  @son-fn="fn" ></child>
       </div>
   ```

   

2. 子组件使用 `@click` 或其他事件触发，在事件中使用 `this.$emit` 去调用父组件中的事件，同时，这里可以传子组件的参数，实现子组件传递数据给父组件。

   父组件中的方法：

   ```JS
       methods: {
           fn(s) {
               this.msg = s;
               console.log('父组件的fn方法');
           }
       }
   ```

   

   子组件通过自定义的事件获取到了父组件中的方法，在 `template` 使用 `@click`去调用：

   ```JS
       methods: {
           data() {
               return {
                   sonMsg: '巴拉巴拉巴拉'
               }
           },
           fnn() {
               console.log(this.$refs);
               this.$emit('son-fn', this.sonMsg);	//重点
           }
       },
       props: ['app-msg'],
       template: '<p @click="fnn"> ----- 这是子组件的P标签-----{{ appMsg }} </p>'
   ```

   

   this.$emit` 参数是第一个为方法名称，这个是在 DOM 中自定义的方法名，后面为传入的参数。

### 我的理解：

- 首先是父组件传递数据给子组件，因为在 `<div id="app">` 标签中就是在父组件中，此时你可以传入任何父组件里面的数据，而子组件自定义的指令接受到了，相当于获取到了传递的值。

- 子组件传递父组件使用的可以认为是回调函数的一种方法，然后通过传入参数去把子组件的参数传递给父组件当中。具体看图：

  ![父子组件通信](/medias/images/父子组件通信.png)

## 兄弟组件以及复杂关系之间的通信

### 子传父，然后父传子

### VueX

### 中央事件总线

前两种在官方中很详细，所以这里重点简述一下中央事件总线，因为 `$on和$emit` 需要在同一公共的实例才能触发，所以我们可以创建一个 Vue 实例专门去放置事件

1. 创建中央 `var bus = new Vue()`

2. 在兄弟组件 A 中添加 `mounted`生命周期函数，挂载点挂载的时候自动开启监听，这里相当于把组件 A 的函数传递给 `bus` 中

   ```JS
   mounted(){
       bus.$on('fn',(val)=>{
           this.name = val;
       })
   }
   ```

   

3. 在兄弟组件 B 中调用 `$emit('fn', this.name)`方法，并且传入组件 B 中的参数，实现兄弟组件或者复杂组件之间的通信

   ```JS
   template: `<div><p> {{ name }} </p><button @click="fnn">点我</button></div>`,
       methods: {
           fnn() {
               bus.$emit('fn', this.name);
           }
       }
   ```

   

## 插槽

> 在组件的标签内容中，填写任何内容，都会被 `template` 模板内容所覆盖，但是有时候我们需要的确实另外的效果，希望标签内的内容能够被保留，这时候就需要用到插槽。

```HTML
<hello>
	这是插槽内容
</hello>
```

在 `template` 中添加 `<slot></slot>` 标签，组件渲染的时候，`<slot></slot>` 就会被替换成 "这是插槽内容"。

```JS
template: "<div> <p>这是一个p</p> <slot></slot>  </div>"
```

并且，在模板内容中的 `slot` 标签中你可以添加任意 HTML 元素，甚至是其他组件，模板标签内的内容就叫做后备内容。当你组件中没有写任何内容的时候，就会渲染出这些内容，可以理解为备用内容。新语法中 `template` 才可以使用 `v-slot` 。

### 编译作用域

> 当你想在插槽中使用数据的时候，你可以要注意到此时的编译作用域问题。
>
> **官方提示：父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的。**

```HTML
 	 <div id="app">
          <hello>
              <p>你们慢慢木</p>
          </hello>
	</div>
```

```JS
 Vue.component('hello', {
            data () {
                return {
                    msg: '666'
                }
            },
            template: `
            <div>
                <slot name="up"></slot>
                <button>123</button>
                <slot name="down"></slot>
                <slot>{{ msg }}</slot>
            </div>
            `
        })
```

这个时候因为 msg 的使用是在子组件的 `template`中调用编译的，所以就是子组件中的 msg。相反，如果你在 `div#app` 标签中使用 `{{ msg }}` 那么编译的就是父组件中的 msg，这一点很重要，可以参考父组件向子组件传递数据，子组件通过自定义指令接受，自定义指令的等号后面就是父组件的内容。

### 具名插槽

> 顾名思义，这是一个有着具体名称的插槽，可以认为是有钥匙和锁对应的插槽。
>
> 在正常情况下，插槽内容都会被加入到插槽模板标签中，也称为默认插槽。

```HTML
 	 <div id="app">
          <hello>
              <p v-slot:cha>你们慢慢木</p>
              <p >巴拉巴拉的</p>
          </hello>
	</div>
```

此时的第一个 P 标签就被上锁了，在子组件的模板内容中直接使用 `slot` 是获取不到的，因为你没有钥匙，钥匙就是在 `slot` 标签上面添加 `name="cha"`，对应插槽内容中，这就叫做具名插槽。

## 非 props 特性

> 什么是 props ？
>
> 当父组件向子组件传递数据的时候，子组件需要使用 `props` 选项去接收传递过来的数据，但是如果父组件传递下来，但是子组件并没有使用 `props` 去接收，那么这些传递过来的属性就会有一些特性。

```HTML
    <div id="app">
        <hello :age="age" :name="name"></hello>
    </div>
```

```JS
        Vue.component('hello', {
            // inheritAttrs:false,//非props特性不让你覆盖  
            template: `<div  name='qqq'>
                          <p> {{ msg }} {{ $attrs }} </p>
                      </div> `,
            props: {
                age: {
                    type: String
                }
            },
            data() {
                return {
                    msg: '这是个 msg ',
                    name: 'lemon'
                }
            }
        })
```

```
这是个 msg { "name": "qfl" }   -----输出的结果
```

此时的 name 就是非 props 特性：

- 能够被 `$attrs` 获取到

- 非props特性自动写入根元素上覆盖，渲染出来的子组件标签上面的 `name`不是 qqq ，而是传递过来的父组件中的 name 值

  非 props 特性会覆盖子组件模板 `template`上面相同的值。

  **注：`class` 和 `style` 两大流氓不受影响，会合并。**

- 通过 ` inheritAttrs:false`选项可以拦截不让 name 覆盖，但是还是能被 `$attrs`，这里是不受影响的。

### 能做些什么呢 ？

> 当我们不想传递到子组件的根元素使用，而是想子元素使用这些属性的时候，并且属性较多，我们不想一个一个在 `data` 中去 return ，繁琐而且容易遗漏出错。
>
> `inheritAttrs:false `和 `$attrs` 可以实现让子组件模板里的子标签能够使用。在需要使用的地方，比如上面的子组件的 P 标签中，我们不需要一个个去写，直接 `v-bind="@attrs"`，注意这里不能简写成 ：。
>
> 注意： `inheritAttrs: false` 选项**不会**影响 `style` 和 `class` 的绑定。

## 思考

### BUS 的原理是什么？

> 我的理解就是，通过中央事件管理器存放监听的方法，然后执行的时候再到这里来调用

```JS
    var fun = {};	//存放各种方法
    var bus = {	//中央处理
        $on: function (eventName, callback) {
            if(!fun[eventName]){	//不存在创建
                fun[eventName] = [];
            }
            fun[eventName].push(callback);
        },  
        $emit: function(eventName, params){
            if(fun[eventName]){	//存在方法
                fun[eventName].forEach(cb => {
                    cb(params);	//
                })
            }
        }
    }
```



### v-model 的原理是怎样的呢？

> 首先是通过 `:value="value"` 指令获取到 data 中的 value 值，实现单向的 value 绑定输入框内容显示。然后再通过事件 `@input="value=$evnet.target.value"` 当你输入值，调用事件修改 data 中的 value 值实现双向绑定。这里需要注意的是单选框和复选框用的是 `@change`。



### 修饰符 sync 语法糖的原理

### 内置组件有哪些？

> `component`
>
> `template`
>
> `slot`