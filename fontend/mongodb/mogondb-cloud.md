# mogondb-cloud

|          | mysql  | mongodb    |
| -------- | ------ | ---------- |
|          | DB     | DB         |
|          | table  | collection |
| 存储方式 |        | json       |
|          | 关系型 | 非关系型   |
|          |        |            |
|          |        |            |
|          |        |            |
|          |        |            |
|          |        |            |

## 初始化

> 免费的有512M的空间存储数据

1、进入[云服务器](https://www.mongodb.com/cloud)创建账号，并选择就近的服务器初始化项目。

​	①创建`project`、创建`cluster`【选免费的，找一个离得比较近的服务器】然后等待初始化

​	`cluster`对应数据库

​	`collection`对应`table`

​	`row`对应 `document`

1.5、如何删除集群和项目

>  删除项目前需要关闭集群

<img src="https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201109154546234.png" alt="delete cluster" style="zoom: 50%;" />



删除项目

<img src="https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201109154627253.png" alt="project settings" style="zoom:67%;" />

```
往下面拉找到delete
```

2、连接数据库，（允许任何ip访问：`allow access from anywhere`）

​	创建db user 和password【让他自己生成】

​	①连接方式 ：1、 shell 2、使用程序

​	

​	**JSON的数据格式必须使用双引号而不能用JavaScript的单引号**

​	新版获取地址会出问题，所以获取旧版本的uri【旧版可用向新版兼容】，在进行操作

```javascript
const MongoClient = require('mongodb').MongoClient;
const uri = "mongodb://zoulam:DUeFDxP25mDawsW@cluster0-shard-00-00.3rise.mongodb.net:27017,cluster0-shard-00-01.3rise.mongodb.net:27017,cluster0-shard-00-02.3rise.mongodb.net:27017/<dbname>?ssl=true&replicaSet=atlas-8lqeco-shard-0&authSource=admin&retryWrites=true&w=majority";
const client = new MongoClient(uri, { useNewUrlParser: true });
client.connect(err => {
  if (err) {
    console.log(err);
  } else {
    console.log('connect success');
  }
  const collection = client.db("test").collection("devices");
  collection.insertOne({
    "devId": 1,
    "name": "devices1"
  }, function (err) {
    if (!err) {
      console.log('insert success')
    }else{
      console.log(err);
    }
  })
  client.close();
});


```

### 除了用代码操作之外

> 直接导入`json`文件



## 云函数

### ①初始化并完成代码

[腾讯云](https://cloud.tencent.com/)

1、创建账号

2、搜索**云函数**并进行基本配置、进入控制台封装代码，有免费额度

<img src="https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201110000528039.png" alt="创建函数" style="zoom: 67%;" />

<img src="https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201001145107914.png" alt="本地写好代码并上传" style="zoom:67%;" />

```javascript
// ------------------------index.js-------------------------------------
'use strict';
const { searchArticle, getAll } = require('./mogondbAPI')
exports.main_handler = async (event, context) => {
    console.log(event.queryString);
    let type = event.queryString.type
    let res = {}
    if (type == '1') {    // name=node.js 文章&type=1
        let name = event.queryString.name
        res = await searchArticle(name)
    }else if (type == '2') {     // type=2
        res = await getAll()
        res = res.map(item => item.name)
    }
    return res
};
```

### ②创建触发器

> 1、api网关触发
>
> 2、get请求

### ③出现错误

通过日志查询模块查看

### ④测试

![用于测试http请求](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201110002155860.png)

