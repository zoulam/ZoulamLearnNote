# \[webpack\]手写

## webpack02

### 此处是webpack原理分析

> 默认cb是`callback`的缩写

### 发布订阅的理解

**订阅**：将函数存入栈中

**发布**：从栈中取出函数执行，钩子的不同取决于发布的方式

### Tapable

> ​ webpack本质是一种事件流机制，将插件串联起来，实现这个功能的核心就是`Tapable`,这类似于`nodejs`内的`events`库,核心原理也是依赖**订阅发布**模式。
>
> ​ 即安装某种特定的顺序执行代码，完成预期的工作。

Tapable在`webpack\lib\Compiler.js`目录下

```javascript
const {
    SyncHook,
    SyncBailHook,
    SyncWaterfallHook,
    SyncLoopHook,
    AsyncParallelHook,
    AsyncParallelBailHook,
    AsyncSeriesHook,
    AsyncSeriesBailHook,
    AsyncSeriesWaterfallHook 
 } = require("tapable");
```

同步注册 `tap` 同步执行`call`

#### SyncHook

`forEach`

同步钩子，顺序执行

#### SyncBailHook

`do while`

​ 保险（bail）钩子，上一次的执行必须返回 `undefined`，否则不会执行后面的函数，即**可以实现**出现了错误的时候阻止执行。

#### SyncWaterfallHook

`reduce`

​ 瀑布钩子，上一个函数的返回值可以被下一个函数以参数的形式接收，产生联系。获取上一步的执行的信息，再做出判断，比起保险钩子更准确。

#### SyncLoopHook（webpack中没有使用）

`forEach + do while`

> 只要某个函数不返回 `undefined` 就会循环执行

## 钩子API命名

同步注册 `tap` 同步执行 `call`

异步注册 `tapAsync` 异步执行 `callAsync`或 `tapPromise` 注册 `promise` 执行

### 并行钩子

#### AsyncParallelHook

> 异步执行，使用计数器计数，当钩子数**全部执行完**之后才执行 `callAsync`内的方法

实现：`pop` 取出最后一个函数，当前面的函数执行完再执行

​ 或者使用`Promise.all`实现

#### AsyncParallelBailHook

> 异步并行的保险钩子， promise reject 或者 返回 非 `undefined` 时终止执行

### 串行钩子（前后有依赖关系）

#### AsyncSeriesHook

普通版与express中间件机制类似，promise版跟redux类似

> 异步的串行钩子，前面的回调函数**一个一个执行完**后面的函数才执行

普通版：中间函数（`next`）+递归

promise 版 ：`shift`+`reduce`

#### AsyncSeriesBailHook

#### AsyncSeriesWaterfallHook

异步串行瀑布钩子

## 手写webpack

### 打包

![link](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200923214049396.png)

将代码link到 `node_modules`文件夹下， 使用`npx [fileName]`就可以直接使用

编译就由babel解决了

> babylon:将源码转成AST babel-tarvers:遍历节点 @babel/traverse babel-types:替换节点 @babel/types @babel/generator:生成代码

`npm i -S babylon @babel/traverse @babel/types @babel/generator`

[AST在线解析](https://astexplorer.net/)

![&#x53D6;&#x51FA;require&#x7684;&#x8FC7;&#x7A0B;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200923232219612.png)

![&#x8DEF;&#x5F84;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200923232736798.png)

#### 模板

> **记得把注释删掉**

```text
(function (modules) {
var installedModules = {};
// 实现__webpack_require__方法
function __webpack_require__(moduleId) {
if (installedModules[moduleId]) {
return installedModules[moduleId].exports;
}
var module = installedModules[moduleId] = {
i: moduleId,
l: false,
exports: {}
};
modules[moduleId].call(module.exports, module, module.exports, __webpack_require__);
module.l = true;
return module.exports;
}
return __webpack_require__(__webpack_require__.s = "<%-entryId%>");
})
// 这是立即执行函数的超长参数以键（路径）值（一个文件）对存在的内容
({
<%for(let key in modules){%>
<!-- key  -->
"<%-key%>":
<!-- value -->
(function (module, exports, __webpack_require__) {
eval(`<%-modules[key]%>`);
})
<%}%>
});
```

### loader

### plugin

## 手写loader

loader分类

`enforce` 配置 `'p're' 'normal' 'post'`

执行顺序：`pre normal inline(行内) post`

**行内loader配置方式**

```javascript
// 添加全部loader
// let ans = require('inline-loader!./a.js')
// -! 就是拒绝 pre 和 normal loader处理
// let ans = require('-!inline-loader!./a.js')
// !  拒绝normal loader处理
// let ans = require('!inline-loader!./a.js')

// !!  只要行内loader
let ans = require('!!inline-loader!./a.js')
```

### loader的组成

`pitch` `normal`

#### pitchloader没有返回值的情况

![pitch&#x6CA1;&#x6709;&#x8FD4;&#x56DE;&#x503C;&#x7684;&#x60C5;&#x51B5;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200925112736382.png)

#### pitchloader有返回值的情况

![pitchloader&#x6709;&#x8FD4;&#x56DE;&#x503C;&#x7684;&#x60C5;&#x51B5;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200925112856225.png)

### 实现babel-loader

### 实现css-loader

> 将代码分割，把`url` 语法 转化为 `require`语法

![css-laoder](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200925162814955.png)

解析结果

```javascript
let list = []
list.push("body {\n  background: red;\n  background: ")
list.push('url('+ require + './public.jpg' + ')')
list.push(";\n}\n")
module.exports = list.join('')
```

转化成 `url(reuqire('./public.jpg'))`

## 手写插件

### 打印文件大小的插件

```javascript
/**
 * 在指定文件存储文件的 文件名文件大小
 */
class FileListPlugin {
    constructor({ filename }) {
        this.filename = filename;
    }
    apply(compiler) {
        compiler.hooks.emit.tap('FileListPlugin',
            /**
             *
             * @param {*} compilation 大量的文件信息
             * @param {*} cb
             */
            (compilation) => {
                let assets = compilation.assets;
                let content = `## 文件名    资源大小\r\n`
                // [[bundle.js,{}],[index.html,{}]]
                Object.entries(assets).forEach(([filename, statObj]) => {
                    content += `- ${filename}   ${statObj.size()}\r\n`
                });
                // 资源对象 assets
                assets[this.filename] = {
                    source() {
                        return content;
                    },
                    size() {
                        return content.length;
                    }
                }
            })
    }
}

module.exports = FileListPlugin;
```

