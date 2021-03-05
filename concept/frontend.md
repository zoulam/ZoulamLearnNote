# frontend

## JS

### `BLOB`

巨量的二进制对象，`binary large object`

### `DOM` 

文档对象模型`document object model`

### `BOM`

浏览器对象模型`browser object model`

### `canvas`

 画布（标量图）

`WEBGL`

`Web Graphics Library`

**下图来自高程第四版PDF**

<img src="https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201209231632497.png" style="zoom:50%;" />

### [SVG](https://aotu.io/notes/2020/06/09/strong-svg/)

`Scalable Vector Graphics` 可缩放矢量图形，使用`XML`格式定义图形。

### `throttle`

节流（技能cd），函数在限定时间内触发多次只执行一次。

### `debounce`

防抖（回城），函数在限定时间内频繁触发不执行，不再触发时开始计时准备执行。

### `HOF`

`higher-order function`，接收回调函数为参数的函数。

###  `css`

（`Cascading Style Sheets`)层叠样式表。

### `CSSOM`

对象模型`css object model`

### [Polyfill](https://developer.mozilla.org/zh-CN/docs/Glossary/Polyfill)

中文释义：一种舒适的衣物，一块代码，旧版浏览器对新版api的代码支持，如使用es5实现数组的函数等。

### `component`

组件（可复用，可拆分），当前的前端开发就是以组件串联的，页面是一个大组件，各个模块是小组件。

### `raf`

``requestAnimationFrame`允许控制动画渲染以获得更优性能。

### `SEO`

搜索引擎优化`Search Engine Optimization`

### `SSR`

[server side render](https://zhuanlan.zhihu.com/p/270149478)，服务端渲染。

### pseudo

`pseudo class` 伪类

`pseudo element` 伪元素

### closure

闭包

### scope

作用域

`scope chain` 作用域链

### context 

上下文

## prototype chain

原型链

**服务端渲染比起客户端渲染的优势**

- 网络链路

- - 省去了客户端二次请求数据的网络传输开销
        - 服务端的网络环境要优于客户端，内部服务器之间通信路径也更短

- 内容呈现

- - 首屏加载时间（FCP）更快
        - 浏览器内容解析优化机制能够发挥作用

### `CSR`

`Client-side rendering`客户端渲染。

### `PWA`

[Progressive web apps](https://developer.mozilla.org/zh-CN/docs/Web/Progressive_web_apps)渐进式web应用。

### `AJAX`

`asynchronous JavaScript And XML`异步的`JavaScript`和 `XML`请求。

### `SPA` 

`single page web application`单页web应用。

### `CDN`

`Content Delivery Network`（内容分发网络）。

### [base64](https://baike.baidu.com/item/base64)

前端中用于将**二进制文件转换成字符串**嵌入HTML文章用于减少`http-request`的方式。

### `Authorization`

授权通常用于存储`token`的字段。

### `JWT`

`Json Web Token`，`http`请求的权限验证，即不携带`token`（服务端生成的**有时效性的唯一的不可见的**）发送的`http`请求会失败，用于避免`csrf`。

### `CSRF`

``Cross-site request forgery`跨站请求伪造，通过伪造的网站盗取用户信息向真正的服务端发送请求，可以通过`referer`和`jwt`避免。

### `i18n`

`internationalization` 国际化的意思。

### `interceptor`

拦截器，如发送request显示`loading组件`，`response`的时候关闭`loading组件`，或者对response的数据进行重复性加工的时候做出抽离操作。

## 打包

### webpack

`web package` web打包工具

### glup

前端开发的自动化工具，跟webpack类似

### [rollup](https://www.rollupjs.com/)

中文意思是卷起，也是打包工具

### [vite](https://github.com/vitejs/vite)

`vue`作者尤雨溪开发的，开发模式下的打包工具。

### `HMR` 

hot module reload  **webpack的热模块加载**

### `HPR`

 hot page reload  **Angular就是这种更新方式**

## **react**

官网文档里我不认识的单词

```
painless 无痛的；轻松的；
phase 阶段
schedule 计划
fiber 中文意思是纤维，可以理解为细小的颗粒，即拆分细小结构的架构
reconcile 调节， reconciler 就是调解器
concurrent 并发，这并不是react的概念；而是一种编程理念，通常指一个进程内的多个线程同时执行
enqueue 加入队列
traverse 遍历
expiration 过期
legacy 遗留；剩余未处理
effect 影响；此处指函数编程的副作用；即对外部环境造成的影响
circular 循环；圆形
suspense 中文意思是悬念；悬疑的意思；在此处可以理解为是待定的意思
（出现在react的生命周期中的生词）
receive 接收
derived 衍生；派发下来的；
snapshot 快照；
```

### dom-diff

### virtual dom

虚拟dom节点



## **vue**

### redirect

指令，前端领域中常常表示出现在`vue`中的某些api，`v-on`等就是vue指令。

### MVVM

并非`vue`首创，`react`只是视图层的框架，而不是别人口中的MVVM框架。

​	简单的理解为 ：

​		用户输入 M 数据模型；在Vue中即Option API中的参数；

​		并且输入V即视图模型；在Vue中即是 `<template>` 标签；最多再加上`css`

​		Vue框架就给你渲染成 VM

## **Angular**

## PP

`Programming paradigm`编程范式

### functional

函数式编程，指使用函数来代替复杂的`for loop`、 `if -else`等操作，达到代码高度复用的编程范式，是好是坏不做评价。

### pure function

纯函数，指返回值只跟传入的参数有关的函数，不会对外部和内部造成任何副作用的函数。

## BFF

>  Backend For Frontend，服务于前端的后端架构，即设计后端接口的时候会考虑前端。