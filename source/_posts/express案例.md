---
title: express案例
date: 2018-05-22 13:05:06
author: One Lemon
img: /medias/featureimages/3.jpg
summary: 
tags: 
	- express
	- Node
	- demo
---

# 通过 express 框架实现简单的小案例

## 需要用到的工具

- [Insomnia](<https://insomnia.rest/download/>)	模拟发送请求的工具

- [Robo 3T](https://robomongo.org/download)	操作 [mongodb](https://www.mongodb.com/download-center/community) 数据库的可视化工具(PS：安装数据库最后左下角不要勾选)

- express 框架

  ```powershell
  npm i --save express
  ```

- 为了能够更好地浏览功能层次，我将文件夹的目录创建如下：

  ![目录](/medias/images/path.png)

## 案例流程

1. 创建server.js文件

   - 这里需要先了解`userRouter`和中间件

   - POST 请求的数据，将会在`req.body`中使用

```js
const express = require('express')	//引入模块
const path = require('path')
const app = express();	//实例化对象
const userRouter = require('./routes/user');//引入分离的路由文件
//中间件	POST请求数据以JSON输出，否则会得到undefined
app.use(express.json());
app.use(express.urlencoded({
    extended: false
}));
app.use('/api', userPouter);
app.listen(4444);
console.log('服务启动成功，请访问http://localhost:4444');
```

2. 在 routes 文件夹中创建 user.js
   - 分离代码，使框架更加清晰，将回调函数存放在`controller/userCtrl`
   - 访问地址是`http://localhost:4444/api/reg/`进入注册页面
   - 如果代码不暴露，即使调用了该文件，也不能直接调用内容方法

```js
const express = require('express');
const router = express.Router();
const UserCtrl = require('../controller/userCtrl');
router.post('/reg', UserCtrl.reg);
module.exports = router	//暴露  用于server.js使用
```

3. 在 controller 文件夹中创建 userCtrl.js
   - 主要是存放不同操作的回调函数
   - 这里需要和数据库交互，所以要引入`model/user`
   - bcrypt 是加密模块，用于加密用户密码 `npm i bcrypt`
   - findOne 返回的是promise，所以使用then()，并且根据返回的数据，如果查询到该用户名在数据库中存在的话data不为空，报错给前端，结束注册请求

```js
const UserModel = require('../model/user');
const bcrypt = require('bcrypt');
const reg = (req, res)=>{	//用户注册
    let name = req.body.username;	//获取用户名，和数据库进行对比
    UserModel.findOne({	//mongodb数据库查询语句
        username: name	
    }).then(data)=>{
        if(data){
            res.send({code: -1, msg: '用户名存在，请更换用户名'});
            return;
        }
        //对象的assign方法来更新对象中的password数据
     	let body = Object.assign({}, req.body, {
            //密码进行哈希加密
            password: bcrypt.hashSync(req.body.password, 10)
        })
        let user = new UserModel(body);	//实例化对象
        user.save().then(()=>{
            res.send({code: 0, msg})
        }).catch(err => {
            console.log(err.message);
            res.send({code: -1,  msg: '注册失败' });
        })
    }
}
```

4. `model/user.js`和`config/db.js` 分别是对进行表操作和数据库连接
   - 在`model/user.js`引入 db.js ，相当于在连接数据库基础上进行表的操作
   - 因为某些原因，这里操作的是 user ，实际操作的是users表

```JS
const db = require('../config/db');
const schema = new dbSchema({	//这里是表的字段名和要求
    username: {
        type: String,
        required: true	//设置用户名为必填项
    }，
    password: {
        type: String,
        required: true	//设置用户名为必填项
	}
})
module.exports = db.model('user', schema);	
```

```js
const mongoose = require('mongoose');
const url = 'mongodb://localhost:27017/apple';
mongoose.connect(url, {
    useNewUrlParser: true
}).then(()=>{
    console.log('数据库连接成功');
}).catch(error => {
	consolelog('数据库连接失败', error.message);
})
module.exports = mongoose;
```

---

## 执行结果

1. 在终端中输入`nodemon server.js`启动服务

2. 在 Insomnia 中进行 POST 请求![POST](/medias/images/POST.png)

----

Tips：一个简单的用户注册就完成了，用户登录以及修改等操作也可以照葫芦画瓢，最重要还是要了解流程

[github案例仓库](https://github.com/One-Lemon/express-demo)