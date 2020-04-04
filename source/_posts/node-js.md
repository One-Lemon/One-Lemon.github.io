---
title: node 基础模块
date: 2019-05-20 10:54:12
author: One Lemon
img: /medias/featureimages/2.jpg
top: false
cover: false
summary: node 基础模块的简单示例，以及express模块使用
tags:
  - node
  - js
---

## node 运行 js文件（PS：安装好了node）

- 在vscode中随便写一段js代码

``` javascript
var x = 200;
console.log('mr的x的值为：' + x);
```

- 在vscode中打开终端

``` powershell
node ./mr.js
```

## fs模块使用

1. 在JS文件中先引入模块 

   ```javascript
   const fs = require('fs');
   ```

2. 使用模块中的方法

   ```js
   fs.readFile('./test.txt', (err, data)=>{
          if(err){	//如果存在异常，报错
           console.log(err);
          }else{	//正常执行输出内容
              console.log(data.toString());
          }
      }
   ```
   
   - bug1：cannot find module	注意路径
   
   - bug2：乱码	输出内容忘记添加toString方法

   ## http模块的使用

1. 在JS文件中引入模块（这一步是万年不变的，）

   ```js
   const http = require('http');
   ```

2. 创建服务以及逻辑（server中可以更根据不同的需求进行逻辑判断）

   ```js
   const server = http.createServer((req, res)=>{
       console.log(req.url);	//终端中打印出地址 favicon.icon忽略
       res.write('123456')；//页面中输出
       res.end()//结束
   })
   server.lister(4242);	//自定义端口号  开启服务
   console.log('服务已开启，请访问http://localhost:4242');
   ```

## express模块

> 高度包容、快速而极简的 [Node.js](http://nodejs.org/) Web 框架
>
> 在http模块中如果深入使用，你会发现许多得问题，例如get和post请求的不同，文件路径的改变。
>
> 监听文件变化，修改代码自动重启服务，降低重复操作

1. 使用方法还是先引入模块（省略，参考上面）

2. 实例化对象

   ``` js
   const app = express();
   ```

3. 请求方法

   ```js
   app.all('/', (req, res)=>{
       let _url = url.parse(req.url);	//需加入url模块
       console.log('访问量+1');
       console.log(_url.query);	//获取到url中的参数
       res.send('hello node js');	//等价于res.write和res.end
   })
   ```

4. 部署静态文件托管

   ```js
   app.use('/', express.static(path.resolve(__dirname,'./public')));
   app.listen(4242);	//监听服务
   console.lg('服务开启，请访问http://localhost:4000');
   ```

### 输出结果（PS：参数是我在地址自己加的）

![1558355610640](C:\Users\lemon\AppData\Roaming\Typora\typora-user-images\1558355610640.png)

### node 模块的时候基本都是异曲同工，需要的是能看懂API手册

## 思考：app.all和app.use有什么区别?

>类似于app.METHOD的路由请求 。
>
>参数：path	路径，callback	回调函数

结合API文档以及实际操作得出的区别

- use通常是作为中间件使用的	例：

  ```js
  app.use('/a', (req, res) => {
      console.log('访问量+1');
  })
  ```

  此时可以接受匹配到/a所有的请求 /a/b也可以。![1558357897282](C:\Users\lemon\AppData\Roaming\Typora\typora-user-images\1558357897282.png)

- all是以具体的路由	例：

  ```js
  app.all('/a', (req, res) => {
      console.log('访问量+1');
  })
  ```

  可以接受/a get ，/a post 等，但是不能路由/a/b。

### 总结：use是中间件，可以路由以path开头的所有请求，但是all是具体的路由，无法路由下级地址的请求。

