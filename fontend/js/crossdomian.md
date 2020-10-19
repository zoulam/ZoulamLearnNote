# \[js\]跨域

> 定义：非同源策略请求，（协议、域名、端口）中的任何一个不同就是跨域
>
> |  | [http://aa.bb:80](http://aa.bb:80) | [https://aa.bb.cc:8080](https://aa.bb.cc:8080) |
> | :--- | :--- | :--- |
> | 协议 | http | https |
> | 域名 | aa.bb\(localhost\) | aa.bb.cc\(127.0.0.1\) |
> | 端口号 | 80 | 8080 |
>
> 为什么要处理跨域问题：
>
> 1. 在前后端分离的场景，前端代码和后端代码往往就存在跨域问题，所以联调时需要解决
> 2. 实现异步无刷新操作，即：不用刷新整个页面就可以从服务器请求到信息（如：验证账号密码）
> 3. 实现服务器拆分提高性能
>
>    web服务器：静态资源（前端）
>
>    data服务器：业务逻辑和数据分析
>
>    图片、视频、音频服务器：这些内容请求速度慢
>
> 4. 调第三方接口
>
> 老式前后端开发联调方案（最终还是上传到同一台服务器并非真正的跨域）
>
> xampp
>
> 修改host文件

## 两种发送ajax请求的方式

### Ajax（xhr）

> asynchronous JavaScript and xml 处理同源策略请求
>
> xhr是ajax的实例 `XMLHttpRequest`

### fetch

> 一种请求方式，ES6新增

## 1、jsonp

JSON with Padding，原理：利用HTML部分标签不存在跨域传输和浏览器自动执行全局函数的特性

`<script>` `<img>` `<link>` `<iframe>` ……

优点：方便

局限性：只能发起get请求，因为要执行全局函数，安全性不佳。

衍生场景：

React：父组件的方法以回调函数的方式传入子组件，子组件执行回调函数，实现子组件修改父组件。

Hybird中与ios原生组件通信，通过约定好的伪url通信

实现原理：

![&#x539F;&#x7406;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200814221008696.png)

![&#x7ED3;&#x679C;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200814223400214.png)

## 2、CORS【常用方案】

> ”跨域资源共享”（Cross-origin resource sharing）
>
> axios库替我们实现了前端方面的功能，服务端需要设置`'Access-Control-Allow-Origin'`，下方代码为express示例
>
> ```javascript
> // Access-Control-Allow-Origin设置
> app.use((req, res, next) => {
>     res.header("Access-Control-Allow-Origin", "http://localhost:8000");//允许源 还有一种是填入 *
>     // res.header("Access-Control-Allow-Origin", "*");//允许所有源（就不能携带cookie），不安全
>     res.header("Access-Control-Allow-Headers", "Content-Type,Content-Length, Authorization, Accept,X-Requested-With");
>     res.header("Access-Control-Allow-Methods", "PUT,POST,GET,DELETE,OPTIONS");//允许请求的方法
>     // res.header("X-Powered-By", ' 3.2.1')
>     if (req.method === "OPTIONS") {
>         res.send(200)； //让前端能够快速返回
>     }
>     else next();
> })
> ```
>
> 流程：客户端发送试探性请求方法：OPTIONS，当服务端返回正确信息，再进行真正的请求

```javascript
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <input type="button" value="get请求" class="get"/>
    <input type="button" value="post请求" class="post"/>
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
    <script>
        document.querySelector(".get").onclick=function(){
            axios.get("https://autumnfish.cn/api/joke/list?num=2")
            .then((value)=>{
                console.log(value);
            },(error)=>{
                console.log(error);
            })
        }

        document.querySelector(".post").onclick=function(){
            axios.post("https://autumnfish.cn/api/user/reg"
            ,{username:"lala"})
            .then((value)=>{
                console.log(value);
            },(error)=>{
                console.log(error);
            })
        }
    </script>
</body>
</html>
```

![&#x7ED3;&#x679C;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200814230844794.png)

## 3、http proxy（webpack）

> 配合`webpack` `webpack-dev-server` 使用
>
>  使用express的中间件代码实现

## 4、nginx反向代理

> 实践：nginx中填入配置，将前端和后端两个接口整合到一个新的接口

## 5、postMessage

> 新的api，原理是将内容嵌入iframe

[postMessage](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/postMessage)

`server.js server2.js a.html b.html`

## 6、socket.io

## 7、基于iframe

`document.domian` + `iframe`

缺陷：跨的是同域名不同子域

`window.name` + `iframe`

