# mongodb-express

## mongodb-express

[视频教程（非本人，我也只是从那里学的）](https://www.bilibili.com/video/BV1wf4y1279C)

[MDN 教程](https://developer.mozilla.org/zh-CN/docs/Learn/Server-side/Express_Nodejs)

### 入门介绍

文档格式：类似于 json

![image-20200724091657814](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200724091657814.png)

![image-20200724091746572](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200724091746572.png)

![image-20200724091804520](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200724091804520.png)

![image-20200724091832423](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200724091832423.png)

![image-20200724091915677](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200724091915677.png)

![image-20200724091954728](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200724091954728.png)

默认路径`C:\Program Files (x86)`

![image-20200724092545722](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200724092545722.png)

#### 一些信息：

默认端口：`27017`

`mongondb://localhost:27017`

数据库的简单设计

![image-20200724093254126](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200724093254126.png)

![image-20200724093318118](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200724093318118.png)

### 增删查改

#### 常用命令

| 命令 | 功能 |
| :--- | :--- |
| `db.hlep()` | 查看命令 |
| `show dbs` | 展示所有的数据库 |
| `use <dbName>` | 创建数据库，或者跳转数据库 |
| `db` | 查看当前使用的数据库 |
| `db.postCollection.insertOne({json数据})` | 插入数据**【json格式】** |
| `db.postCollection.find({key:value})` | 参数空查找所有文档，填入则是筛选 |
| `db.postCollection.findOneAndUpdate()` | 跟上面一样，但是会返回更新后的文档 |
| `db.postCollection.updateOne({oldValue},{$set{newValue}})` | 修改一条 |
| `db.postCollection.updateMany()` | 修改多条 |
| `db.postCollection.deleteOne()` | 删除一条 |
| `db.postCollection.deleteMany()` | 删除多条 |

```bash
use myblog
#进入myblog数据库，没有则创建名字为myblog的数据库


db
#查看当前数据库名称


#增
db.postCollection.insertOne({})


#找 key属性的引号是可以省略的
db.postCollection.find({})
#查找普通值
db.postCollection.find({ title: "Mongo Prime"})
#查找对象的值
db.postCollection.find({"author.name":"zoulam"})


#改update，两个参数：1、文档 2、属性
#修改一条
db.postCollection.updateOne({title:"Mongo Prime"},{$set:{title:"MongoDB Prime"}})
db.postCollection.updateOne({"title" : "MongoDB Prime"},{$set:{"title":"MongoDB Prime1"}})
#修改所有满足条件的数据
db.postCollection.updateMany({"author.avatar":""},{$set:{"author.avatar":"https://avatars0.githubusercontent.com/u/55095160?s=460&u=99625dece08b94f87202295ca65a4afbd373bb3a&v=4"}})


#删 delete  参数:ObjectId
 db.postCollection.deleteOne({"_id" : ObjectId("5f1a3d0c46723bebf80eb179")})
db.postCollection.deleteMany()
```

```bash
#添加表一
db.postCollection.insertOne({
    title: "Mongo Prime",
    author:{
        name:"zoulam",
        avatar:"https://avatars0.githubusercontent.com/u/55095160?s=460&u=99625dece08b94f87202295ca65a4afbd373bb3a&v=4"
    },
    createAt:"2020-7-24",
    content:"MongoDB 的小demo",
    comments:[
        {
            user:"lulu",
            comment:"nice"
        },
        {
            user:"lala",
            comment:"unbad"
        }
    ]
})

#添加表二
db.postCollection.insertOne({
    "title": "Node.js Prime",
    "author":{
        "name":"zoulam",
        "avatar":""
    },
    "createAt":"2020-7-24",
    "content":"MongoDB 的小demo",
    "comments":[
        {
            "user":"lulu",
            "comment":"nice"
        },
        {
            "user":"lala",
            "comment":"unbad"
        }
    ]
})
```

![image-20200724094149106](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200724094149106.png)

![image-20200724101311237](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200724101311237.png)

[mongodbGUI 操作](https://robomongo.org/)

## Express+MongoDBAPI

MVC：model view controller

MVP：model view presenter（松散的控制器，中文意思有：主持人，主要角色）

MVVM：model view view model （现在主要应用在前端领域，Vue、React（反应、响应）、Angular）

[express 官方网站](https://expressjs.com/)

### ①install mongodb driver and config

```bash
npm i express mongodb --save
```

#### 封装mongodb

1、向外暴露连接好的 `getCollection()` 执行即返回 `postCollection()` 函数

2、封装增删查改操作

3、基于express使用http请求执行增删改查操作

```javascript
const MongoClient = require("mongodb").MongoClient;

const url = "mongodb://localhost:27017";
const dbName = "myblog";
let _db = null;

async function connectDb() {
  if (!_db) {
    const client = new MongoClient(url, { useUnifiedTopology: true });
    try {
      await client.connect();
      _db = await client.db(dbName);
    } catch (error) {
      throw "连接到数据库出错";
    }
  }
  return _db;
}

exports.getCollection = (collection) => {
  let _col = null;
  return async () => {
    if (!_col) {
      try {
        const db = await connectDb();
        _col = await db.collection(collection);
      } catch (error) {
        throw "选择 collection 出错";
      }
    }
    return _col;
  };
};
```

### ②express 简要介绍

![&#x6700;&#x5C0F;&#x53EF;&#x7528;&#x6846;&#x67B6;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200729103018193.png)

> 项目创建
>
> ```bash
> mkdir express-prime
> cd express-prime
> npm init -y #yarn init -y
> npm install express#yarn add express
> code .  #使用vscode打开代码
> ```

| 方法（`const app = express()`） | 效果 |
| :--- | :--- |
| request:请求 response：响应 |  |
| `app.listen(port,()=>{})` | 指定监听端口 |
| `app.use(express.json())` | 使用中间件 |
| `app.get("/",(req,res)=>{res.send('')});` |  |
| `app.post();` |  |
| `app.put("/:id",()=>{});` |  |
| `app.delete("/:id",()=>{});` |  |
| `res.send()` | 发送响应内容 |
| `res.status(201).send()` | 发送\[201\]状态码 |
| `res.status(201).json(newPost)` |  |
| `req.body` | 请求体 |
| `req.params.id` | url 设置的`:id` |
| **子路由** |  |
| `let route = express.Router()` | 创建路由 |
| `route.get/post……` | 用路由代替原来的app **需要导出路由变量** |
| `app.use("/post",()=>{})` | 路由以中间件的形式插入回去 |

```javascript
// -----------------------------------app.js---------------------------------------------
const express = require("express");
const app = express()
// const post = require('./routes/post')
const routes = require("./routes")
const port = 3000

app.use(express.json())
// app.use('/post', post)
routes(app)

app.listen(port, () => {
  console.log(`Express server listening at http://localhost:${port}`);
});

// --------------------下面的内容拆分给路由管理---------------------------
app.get("/", (req, res) => {
  res.send("Hello World");
});

app.post("/", (req, res) => {
  console.log("收到请求体：", req.body);
  res.status(201).send();
});

app.put("/:id", (req, res) => {
  console.log("收到请求参数，id 为：", req.params.id);
  console.log("收到请求体：", req.body);

  res.send();
});

app.delete("/:id", (req, res) => {
  console.log("收到请求参数，id 为：", req.params.id);
  res.status(204).send();
});
```

### ③ 子路由（`/routes/post.js`）

| 方法（`var route = express.Router();`） | 效果 |
| :--- | :--- |
| `route.get()` |  |
| `route.post()` |  |
| `route.delete()` | **带id** |
| `route.put()` | **带id** |

```javascript
------------------------------post.js--------------------------------------------
const express = require('express')
let route = express.Router()

const postModle = require('../models/post')

// 查找全部 http://localhost:3000/post
route.get('/', async (req, res) => {
    try {
        const posts = await postModle.findAll()
        console.log(posts);
        res.status(200).json(posts)
    } catch (error) {
        console.log(error);
        res.status(404).send()
    }
})

// 添加 http://localhost:3000/post
route.post('/', async (req, res) => {
    try {
        console.log('get', req.body)
        const newPost = await postModle.save(req.body)
        res.status(201).json(newPost)
    } catch (error) {
        console.error(error);
        res.status(500).send()
    }

})
// http://localhost:3000/post/5fa90c6c64f7102ce0fe0042
route.put('/:id', async (req, res) => {
    try {
        const updatePost = await postModle.update(req.params.id, req.body)
        res.status(200).json(updatePost)
        // console.log("请求id", req.params.id);
        // console.log("请求体", req.body);
        // res.send({ id: req.params.id, ...req.body })
        // res.status(200).send()
    } catch (error) {
        res.status(500).send()
    }
})

// http://localhost:3000/post/5f211e99201ddc315c0ec517
route.delete('/:id', async (req, res) => {
    // :id 会挂载在req.params.id上面
    try {
        const deletePost = await postModle.delete(req.params.id)
        // console.log('请求id', req.params.id);
        res.status(204).send(deletePost)
    } catch (error) {
        res.status(500).send()
    }
})


// http://localhost:3000/post/5f211e99201ddc315c0ec517/commont
route.delete('/:id/commont', async (req, res) => {
    try {
        const deletePost = await postModle.deleteCommentByUser(req.params.id, req.body.user)
        res.status(204).send(deletePost)
    } catch (error) {
        console.log(error);
        res.status(500).send()
    }
})

module.exports = route
```

### ④ 当有多个子路由时

为了防止`app.js`文件过于庞大，在 router 文件夹下创建`index.js`统一挂载子路由

```javascript
//----------------------------index.js----------------------------------
const post = require("./post");
module.exports = (app) => {
  app.use("/post", post);
};
```

### ⑤ 数据库写入时安全性校验。

`commonjs`**引入文件夹，而不是js文件**，会默认导出`index.js`文件的数据

### **⑥总结**

```javascript
// nodejs操作mongodb流程
// 1、 创建数据库实例 【uri可以是本地的，也可以是远程的】
const client = new MongoClient(uri, { useNewUrlParser: true, useUnifiedTopology: true });
// 2、 连接数据库并获取指定collection
await client.connect()
const <collectionName> = client.db(dbName).collection(collectionName)
// 3、 获取postCollection【可省略】并执行相应的操作
let res = await CommonApp.find({})
// 4、 关闭数据连接
client.close()
```

