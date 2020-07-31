---
description: 以HTTP为主要内容
---

# 计算机网络

xhr：XMLhttpRequest

ajax（asynchronous JavaScript and xml）请求

## HTTP概念

1、HyperText Transfer Protocol超文本传输协议

​ www文件都遵循这个标准，指定消息和响应

2、实线web服务器（server）和客户端（client）之间的通信

3、HTPP请求/响应

* 无状态（造成用户信息无法保留，每次访问都要重新发送请求）

  1、每个请求都是完全独立的；

  2、对于事物的处理是没有记忆能力的；

  3、Cookie、Session、Local Storage被用于增强用户体验；

1、HTTPS Hyper Text Transfer Protocol over SecureScoket Layer

2、对发送的数据进行加密（过去会遭遇运营商流量劫持，要交保护费）

2、SSL（安全Scoket协议）/TLS（传输层安全协议）

4、应用于敏感通讯：交易支付等

5、当下一个网站不使用htpps协议就会弹出浏览器的警告

HTTP的请求方法

GET:亲求指定页面的信息，并返回实体主体\(read\)

​

POST:想执行资源提交数据进行处理请求\(create\)

​ 提交form表单、上传的文件等

PUT:从客户端向服务器传送的数据取代指定（文档）或页面的内容（update）

​ 数据更新

DELETE:请求删除服务器的指定页面（delete）

​ 删除信息

……[more](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods)

## http请求/响应报文（字符串文本）

header头body体

请求报文：请求行+请求头+请求体

在chrome devtool内的Network就可以直接观察

1、请求方法

2、请求url

3、HTTP协议及版本

4、请求头（客户端信息）

5、请求体（name=zoulam&password=123456&phoneNumber=123456）

响应报文：响应行+响应头+响应体

1、报文协议及版本

2、状态码及状态描述（200 OK）

3、响应头（客户端的相关信息）

4、响应体（json数据）

### HTTP Header message（部分信息）

General Header

* Request URL
* Request Method
* Status Code
* Remote Address（http请求的远程地址【源地址】）
* Referrer Policy（w3c官方提出的候选策略，主要用作规范referrer）

Requeset Header（出于安全可将这个内容设置为禁止查看）

* Cookies
* Accept-XXX（指定客户端接收的内容类型：字符编码、压缩编码类型、）
* Content-Type（指定内容类型text/html; charset=utf-8、application/javascript; charset=utf-8）
* Content-Length（请求的内容长度）
* Authorization（授权证书）
* User-Agent（用户代理：客户端信息（如：浏览器、操作系统））
* Referrer（链接来源地址）

Response Header

* server（服务器）
* Set-Cookie（设置的cookie）
* Content-Type
* Content-Length
* Date（日期）

注：**GMT**格林尼治标准时间。 在HTTP协议中，时间都是用格林尼治标准时间来表示的，而不是本地时间。（与东八区差8个小时）

### 状态码

1xx请求被接收，需要处理

2xx处理成功，表述服务器已成功接收、理解、并接受

3xx重定向到其他地方，它让客户端再发起一个请求以完成整个处理

4xx处理发生错误（客户端），请求不存在、未被授权、禁止访问的页面

5xx处理发生错误（服务端），服务端抛出异常、路由出错、HTTP版本不支持等

|  |  |
| :--- | :--- |
| 200（OK） | 请求成功 |
| 201（Created） | 请求已经被实线，而且有一个新的资源已经依据请求的需要而建立 |
| 301（See Other） | 重定向到其他地方 |
| 304（Not Modified） | 该资源在上次请求之后没有任何更改（浏览器的缓存机制，即刷新页面是页面内容未变化） |
| 400（Bad Request） | 语义错误/请求参数有错误 |
| 401（Unauthorzied） | 客户端无权访问该资源（不是会员） |
| 403（Forbidden） | 客户端未能获得授权（错误的用户名，密码） |
| 404（Not Found） | 在指定的位置不存在所申请的资源 |
| 500（Internal Server Error） | 服务器遇到了一个未曾预料的状况，导致它无法完成对请求的处理（服务器源代码错误，服务器维护） |

## HTTP/1.1和HTTP/2的区别

**（重点内容、需要查阅博客和翻阅文档）**

2的内容

1. 二进制格式而非文本格式
2. 多路复用

   ![http2.0](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/16cff873bf2ec175)

3. 头部数据压缩
4. 服务器推送

## express的http简单演示

```javascript
const express = require('express');
const path = require('path');

const app = express();

// 中间件对请求体进行解析
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
// 解析public下的静态HTML页面,测试四种请求时要将他注释掉
app.use(express.static('public'));

// 创建get路由 第一个参数是路径，这里的/是根路径
// app.get("/",(req,res)=>{
//   // res.send("this is express");

//   // Content-Type
//   // res.send("<h1>this is express<h1>");//text/html; charset=utf-8
//   // res.send({msg:"zoulam"});//application/json; charset=utf-8
//   // res.json({msg:"zoulam"});//application/json; charset=utf-8

//   // res.json(req.header('host'));//"localhost:4000"
//   // res.send(req.header('host'));//localhost:4000

//   // res.json(req.header('user-agent'));//"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/84.0.4147.89 Safari/537.36"
//   // user-agent："PostmanRuntime/7.26.2"

//   // res.json(req.rawHeaders);
// //   [
// //     "Content-Type",
// //     "application/json",
// //     "User-Agent",
// //     "PostmanRuntime/7.26.2",
// //     "Accept",
// //     "*/*",
// //     "Postman-Token",
// //     "2e9110cb-4dc4-477b-bc1f-a9578be87833",
// //     "Host",
// //     "localhost:4000",
// //     "Accept-Encoding",
// //     "gzip, deflate, br",
// //     "Connection",
// //     "keep-alive",
// //     "Content-Length",
// //     "25"
// // ]

// });


app.post('/contact', (req, res) => {
  //设置中间件就是为了解析这里的req.body
  // res.send(req.body);
  // 验证是否填入name
  // res.send(req.header('Content-Type'));//application/json
  if (!req.body.name) {
    return res.status(400).send('姓名为必填项');
  }
  res.status(201).send(`欢迎${req.body.name}`);
});


app.post('/login', (req, res) => {
  if (!req.header('x-auth-token')) {//x-auth-token是自定义的
    return res.status(400).send('没有令牌');
  }
  if (req.header('x-auth-token') !== '123456') {
    return res.status(401).send('没有权限');
  }
  res.send('登录成功');
});

// 创建博客
app.put('/post/:id', (req, res) => {
  //http://localhost:4000/post/1
  res.json({
    id:req.params.id,//"id":"1"
    title:req.body.title,
  });
});

app.delete('/post/:id', (req, res) => {
  res.json({
    msg: `博客${req.params.id}已经删除`,
  });
});

let port = 4000;
app.listen(port, () => {
  console.log(`url:http://localhost:${port}`);
});
```

* request header
* response body
* SYN：同步序列编号（_Synchronize Sequence Numbers_）

  [syn百度百科](https://baike.baidu.com/item/syn)

* ACK：确认字符（Acknowledge character）

  [ack百度百科](https://baike.baidu.com/item/ACK)

