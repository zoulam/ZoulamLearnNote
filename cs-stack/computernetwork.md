---
description: 以HTTP为主要内容
---

# 计算机网络

xhr：XMLhttpRequest

ajax（asynchronous JavaScript and xml）请求

## HTTP概念

1、HyperText Transfer Protocol超文本传输协议

​	www文件都遵循这个标准，指定消息和响应

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

GET:亲求指定页面的信息，并返回实体主体(read)

​	

POST:想执行资源提交数据进行处理请求(create)

​	提交form表单、上传的文件等

PUT:从客户端向服务器传送的数据取代指定（文档）或页面的内容（update）

​	数据更新

DELETE:请求删除服务器的指定页面（delete）

​	删除信息

OPTIONS:试探性请求

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





###  HTTP Header message（部分信息）

General Header

- Request URL
- Request Method
- Status Code
- Remote Address（http请求的远程地址【源地址】）
- Referrer Policy（w3c官方提出的候选策略，主要用作规范referrer）



Requeset Header（出于安全可将这个内容设置为禁止查看）

- Cookies
- Accept-XXX（指定客户端接收的内容类型：字符编码、压缩编码类型、）
- Content-Type（指定内容类型text/html; charset=utf-8、application/javascript; charset=utf-8）
- Content-Length（请求的内容长度）
- Authorization（授权证书）
- User-Agent（用户代理：客户端信息（如：浏览器、操作系统））
- Referrer（链接来源地址）



Response Header

- server（服务器）
- Set-Cookie（设置的cookie）
- Content-Type
- Content-Length
- Date（日期）

注：**GMT**格林尼治标准时间。 在HTTP协议中，时间都是用格林尼治标准时间来表示的，而不是本地时间。（与东八区差8个小时）





### 状态码

1xx请求被接收，需要处理

2xx处理成功，表述服务器已成功接收、理解、并接受

3xx重定向到其他地方，它让客户端再发起一个请求以完成整个处理

4xx处理发生错误（客户端），请求不存在、未被授权、禁止访问的页面

5xx处理发生错误（服务端），服务端抛出异常、路由出错、HTTP版本不支持等

|                              |                                                              |
| ---------------------------- | ------------------------------------------------------------ |
| 200（OK）                    | 请求成功                                                     |
| 201（Created）               | 请求已经被实线，而且有一个新的资源已经依据请求的需要而建立   |
| 301（See Other）             | 重定向到其他地方                                             |
| 304（Not Modified）          | 该资源在上次请求之后没有任何更改（浏览器的缓存机制，即刷新页面是页面内容未变化） |
| 400（Bad Request）           | 语义错误/请求参数有错误                                      |
| 401（Unauthorzied）          | 客户端无权访问该资源（不是会员）                             |
| 403（Forbidden）             | 客户端未能获得授权（错误的用户名，密码）                     |
| 404（Not Found）             | 在指定的位置不存在所申请的资源                               |
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

```JavaScript
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
  
  
  
  
  
# 面试题

## 1、输入url到页面打开发生了什么？

[博客详解](https://segmentfault.com/a/1190000006879700)

1. DNS解析 域名地址=>ip地址

2. TCP连接 

3. 发送HTTP请求

4. 服务器处理请求并返回HTTP报文

5. 浏览器解析渲染页面

6. 连接结束

### 1.1页面渲染过程

[里面有详细介绍](https://www.html5rocks.com/zh/tutorials/internals/howbrowserswork/)

文字版：

1. ​	创建DOM树（即DOM对象），解析html元素和字符数据，添加element节点和text节点到doucment中。此时，`document.readyState = 'loading'`

2. 遇到link外部CSS，创建线程加载，并继续解析到文档

3. 遇到外部引入的JS

    a.未设置defer、async等属性浏览器加载JS，**并堵塞**，等待JS加载并执行完成，然后继续解析文档

    b. `async`异步加载脚本，**脚本加载完成**立即执行脚本

    c. `defer="defer"` 异步加载脚本， **文档解析完成后**执行脚本

    ​	**注：**异步脚本没有固定的执行时间，但总是在`onload`之前执行

4. 遇到img等，先解析DOM结构，然后异步加载src，并继续解析文档

5. 文档解析完成，此时 `document.readyState =  'interactive'`

6. defer脚本执行

7. document对象触发`DOMContentLoaded`事件，标志着程序执行由同步脚本执行阶段转化为事件驱动阶段

8. 文档和所有资源（img，外部url内容……）加载完成 `document.readyState = 'complete'`, window 触发 `onload` 事件

9. 此后，以异步响应方式处理用户输入、网络事件等。

    ```javascript
    <!DOCTYPE html>
    <html lang="en">
    
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>
    
    <script>
        console.log(document.readyState); // loading
    </script>
    
    <body>
        <div>111</div>
        <script>
            document.addEventListener('DOMContentLoaded', function () {
                console.log(document.readyState);// interactive
            });
            window.onload = function () {
                console.log(document.readyState); // complete
            }
        </script>
    </body>
    
    </html>
    ```

    

## 2、协商缓存和强缓存的区别？

[博客详解](https://segmentfault.com/a/1190000006879700)

[仓库](https://github.com/amandakelake/blog/issues/41)

## 3、HTTP/1.1和HTTP/2.0和HTTP/3.0的区别？

![http2.0新增](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200921120547538.png)

## 4、HTTP和TCP的区别？？？？

http是应用层协议、http是基于

TCP是传输层协议

[TCP报文](https://segmentfault.com/a/1190000015044878)

## 5、https

### 加密方式、原理

[https介绍](https://blog.csdn.net/guizaijianchic/article/details/77961418)

[rfc](https://tools.ietf.org/html/rfc6101)

## 6、get请求和post请求的区别

[关于大小限制的专业解释](https://www.jianshu.com/p/512389822f8b)

get请求的可传输内容有大小限制，post没有

get（在header） post（在body）

## 7、http无状态

> http请求不会保存任何信息（客户端不会记录服务端发送了什么信息，服务端也是不会记录请求）
>
> 【这是为了更快地处理大量事务，确保协议的可伸缩性，而特意把 HTTP 协议设计成如此简单的。】，
>
> 每一次请求都是独立的不会保存任何信息，所以需要cookie、session、localstorage等手段增加用户体验

## 8、websocket

> 比过往建立长连接使用轮询方式更加高效

[websocketMDN文档](https://developer.mozilla.org/zh-CN/docs/Web/API/WebSockets_API)

[socketIO](https://socket.io/)

## 9、Cookie常用设置属性

> Cookie的局限性是大小 `4KB`

![devtools的截图](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200913103318391.png)

  分别是

**name**

**value**

**domian** 生效域

**path** 生效路径路径

**expires/Max-Age** （到期）保留时间

**Size** 大小

**HttpOnly** 只能用Http请求修改（如果不设置用户可以使用JavaScript脚本直接修改）

**secure** 安全 （cookie只应通过被HTTPS协议加密过的请求发送给服务端）

[SameSite可以看这篇文章](https://segmentfault.com/a/1190000022210375)

**Priority** 优先级

## 10、HttpProxy

webpack中就是使用express实现了httpproxy完成跨域

## 11、对称加密和非对称加密

[这篇文章](https://www.cnblogs.com/jfzhu/p/4020928.html)

## 12、cdn原理

简述：将服务器分散在各地，根据用户的位置信息连接不同地区的服务器，达到快速访问和分散服务器压力的效果

[参考文章1](https://www.jianshu.com/p/1dae6e1680ff)

[参考文章2](https://juejin.im/post/6844903873518239752)

## 13、rpc

[wiki](https://zh.wikipedia.org/wiki/%E9%81%A0%E7%A8%8B%E9%81%8E%E7%A8%8B%E8%AA%BF%E7%94%A8)

## 14、ajax原理

## 15、七层（五层）网络模型

[详细介绍](https://blog.csdn.net/qq_22238021/article/details/80279001)

应用				**HTTP**、FTP、NFS、WAIS、SMTP

表示				Telnet、Rlogin、SNMP、Gopher

会话				SMTP、DNS

传输				**TCP、UDP**

网络				IP、ICMP、ARP、RARP、AKP、UUCP

数据链路

物理层

五层是将（应用、表示、会话）合称应用层

四层是将（数据链路和物理层）合称数据链路层（TCP/IP模型）

## 16、拥塞控制

[wiki百科](https://zh.wikipedia.org/wiki/TCP%E6%8B%A5%E5%A1%9E%E6%8E%A7%E5%88%B6)

## 17、tcp和udp

[文件介绍](https://github.com/amandakelake/blog/issues/47)

## 18、三次握手四次挥手

[我觉得比较好的答案](https://www.nowcoder.com/test/question/done?tid=37551893&qid=55067#summary)

> 考点：为什么少了不行，多了也不行
>
> ​	少了成功率低，多了对成功率提升不大（但是对效率影响却大）

[博客介绍](https://blog.csdn.net/qzcsu/article/details/72861891)

[知乎文章](https://zhuanlan.zhihu.com/p/53374516)

## 19、热更新

[webpack热更新](https://juejin.im/post/6844904008432222215#heading-13)

## 20、URL的query、params、hash

协议://域名[ip]/path/?query#hash

域名或者ip或者主机名+端口号

query查询字符串（`?username=zoulam&password=123456`）

params参数（）

hash锚点（定位网页位置 #）

## 21、url解析

中文URL会被转义成特殊格式

```javascript
var myURL = 'https://www.dudu.com?name=张三';
var newURL = encodeURI(myURL);
console.log(newURL);//https://www.dudu.com?name=%E5%BC%A0%E4%B8%89
var backURL = decodeURI(newURL);
console.log(backURL);//https://www.dudu.com?name=张三
var str=decodeURI('%ddss%');//URIError: URI malformed
```

##  22、Upgrade & connection

HTTP1.1特有的**客户端**用于升级协议的字段，当然也有例外：服务端升级传输安全协议（TLS）

[upgrade MDN介绍](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Protocol_upgrade_mechanism)

[connection MDN极少](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Connection)

## 23、Web Workers （一个类）

[mdn介绍](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Workers_API/Using_web_workers)

[mdn API](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Workers_API)

[阮一峰介绍](http://www.ruanyifeng.com/blog/2018/07/web-worker.html)

webworkers是独立于主线程的后台线程，使用场景是一个需要大量时间运行的代码（以避免阻塞【UI进程】主进程）

## 24、Service Worker

[mdn](https://developer.mozilla.org/zh-CN/docs/Web/API/Service_Worker_API)

[MDN介绍如何使用](https://developer.mozilla.org/zh-CN/docs/Web/API/Service_Worker_API/Using_Service_Workers)

## 25、浏览器缓存

[可以从第一篇文章开始看](https://github.com/amandakelake/blog/issues/43)

## 26.uri和url

uri（统一资源标识符）

url（统一资源定位符）

url是uri的子集，都是找到http服务器资源的方式，uri的方式给资源独一无二的id来实现确定资源，而url的实现方式是定位（资源地址或者说是文件路径）

uri的形式：**165481654156615**.htm  加粗部分就是资源独一无二的id（更符合uri的定义）

## 27.cookie 、 sessionstorage、localstorage

常用方式：cookie 记录信息（非明文信息，相当于是一个id，然后从客户端取出session）

服务端的响应报文通过 `set-cookie` 设置，并存储在客户端

可设置属性

![可设置属性](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200921103324678.png)



## 28.状态码

## 29.常见请求方法

![常见请求方法](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200921102645144.png)

##   30.报文格式

行 头 回车+换行 体

回车：定位到下一行的左边界 **(\r)**

换行：纸（打印的时候）下移一行 **(\n)**

## 31.状态码分类

![状态码分类](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200921110633059.png)

## 32.为什么要有https

服务端的dos攻击

客户端的信息泄漏

客户端从服务端下载了被篡改的内容（中间人攻击）

## 33.ssl加密算法

共享密钥加密：传输密钥 （**确保安全的情况下使用**，效率较高）

公开密钥加密：密钥拆分成 公钥和私钥（算法较为复杂，效率较低）

发送密文的一方使用对方的公开密钥进行加密处理，对方收到被加密的信息后，再使用自己的私有密钥
进行解密。

