# \[webpack\]手写

> 默认cb是`callback`的缩写

## 发布订阅的理解

**订阅**：将函数存入栈中

**发布**：从栈中取出函数执行，钩子的不同取决于发布的方式

### Tapable

> webpack本质是一种事件流机制，将插件串联起来，实现这个功能的核心就是`Tapable`,这类似于`nodejs`内的`events`库,核心原理也是依赖**订阅发布**模式。
>
> 即安装某种特定的顺序执行代码，完成预期的工作。

![Tapable](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/bVbrU0E)

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

保险（bail）钩子，上一次的执行必须返回 `undefined`，否则不会执行后面的函数，即**可以实现**出现了错误的时候阻止执行。

#### SyncWaterfallHook

`reduce`

瀑布钩子，上一个函数的返回值可以被下一个函数以参数的形式接收，产生联系。获取上一步的执行的信息，再做出判断，比起保险钩子更准确。

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

或者使用`Promise.all`实现

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

## 搭建环境

在`bin`文件下创建可执行脚本 `MyPack.js`

```javascript
#! /usr/bin/env node
// 当前文件需要使用node环境执行

// 获取webpack.config.js 文件信息
const path = require('path');
const config = require(path.resolve('webpack.config.js'))
const Compiler = require('../lib/Compiler.js');
const compiler = new Compiler(config);
compiler.hooks.entryOption.call();

// 标识运行编译
compiler.run();
```

```javascript
  "bin": {
    "mywebpack": "./bin/MyPack.js"
  },
```

```text
link到全局【源码目录下】
npm link

使用【打包目录下】
npm link mywebpack
npm unlink mywebpack 取消关联

此时就可以使用
npx mywebpack 即可执行当前脚本，在bin那里写了什么就是什么
```

使用的依赖

```text
    // 编译
    "@babel/generator": "^7.11.6",
    "@babel/traverse": "^7.11.5",
    "@babel/types": "^7.11.5",
    "babylon": "^6.18.0",
    // 模板
    "ejs": "^3.1.5",
    // link脚本
    "link": "^0.1.5",
    // 发布订阅钩子函数
    "tapable": "^2.0.0"
```

```javascript
const fs = require('fs');
const path = require('path');
const process = require('process');
const babylon = require('babylon');
const t = require('@babel/types');
// 用es6语法导出的需要加上default,否则引入的就是对象
const traverse = require('@babel/traverse').default;
const generator = require('@babel/generator').default;
const ejs = require('ejs')
const { SyncHook } = require('tapable')
class Compiler {
    /**
     *
     * @param {new Obejct} config 配置文件对象
     */
    constructor(config) {
        // 包含entry 和 output 等参数
        this.config = config;
        // 保存文件入口的路径
        this.entryId;// './src/index.js'
        // 保存模块依赖
        this.modules = {};
        this.entry = config.entry;// 入口路径
        // cwd code work dirname 命令运行的路径
        this.root = process.cwd();
        // 添加声明周期函数
        this.hooks = {
            entryOption: new SyncHook(),
            Compiler: new SyncHook(),
            afterCompiler: new SyncHook(),
            afterPlugins: new SyncHook(),
            run: new SyncHook(),
            emit: new SyncHook(),
            done:new SyncHook(),
        }
        let plugins=this.config.plugins
        if(Array.isArray(plugins)){
            plugins.forEach(plugin=>{
                plugin.apply(this);
            })
        }
        this.hooks.afterPlugins.call();
    }

    /**
     *
     * @param {*} modulePath 源代码模块路径
     */
    getSource(modulePath) {
        // 取出每一个规则做处理
        let rules = this.config.module.rules;
        let content = fs.readFileSync(modulePath, 'utf-8');
        for (let i = 0; i < rules.length; i++) {
            let rule = rules[i];
            let { test, use } = rule;
            let len = use.length - 1;
            if (test.test(modulePath)) {
                function normalLoader() {
                    // 取出loader代码
                    let loader = require(use[len--]);
                    // 递归调用loader,直到取完为止
                    content = loader(content)
                    if (len >= 0) {
                        normalLoader();
                    }
                }
                normalLoader();
            }
        }
        return content;
    }


    /**
     *
     * @param {*} source 源码
     * @param {*} parentPath 父路径
     * @description AST解析语法树
     */
    parse(source, parentPath) {
        let ast = babylon.parse(source);
        let dependencies = [];// 依赖数组
        traverse(ast, {
            CallExpression(p) {
                let node = p.node;
                if (node.callee.name === 'require') {
                    node.callee.name = '__webpack_require__';
                    let moduleName = node.arguments[0].value;
                    // 添加js后缀
                    moduleName = moduleName + (path.extname(moduleName) ? '' : '.js')
                    moduleName = './' + path.join(parentPath, moduleName);
                    dependencies.push(moduleName);
                    // 替换节点修改源码
                    node.arguments = [t.stringLiteral(moduleName)];
                }
            }
        });
        let sourceCode = generator(ast).code;
        return { sourceCode, dependencies }
    }

    /**
     *
     * @param {string} modulePath 工作路径
     * @param {boolean} isEntry 是否是入口
     */
    buildModule(modulePath, isEntry) {
        // 拿到模块内容(代码)
        let source = this.getSource(modulePath);
        // 获取相对路径 即this.root 到模块的路径 ./src\index.js
        let moduleName = './' + path.relative(this.root, modulePath);

        if (isEntry) {
            // 将主入口的路径保存到entryId
            this.entryId = moduleName;
        }
        // 解析需要改造source代码 返回一个依赖列表
        // path.dirname(moduleName) ./src
        let { sourceCode, dependencies } = this.parse(source, path.dirname(moduleName));
        this.modules[moduleName] = sourceCode;
        dependencies.forEach(dep => {
            // 实现递归加载除外部依赖
            this.buildModule(path.join(this.root, dep), false);
        });
    }

    /**
     * 发送编译文件
     */
    emitFile() {
        // 打包后的文件路径
        let mian = path.join(this.config.output.path, this.config.output.filename);
        // 获取代码模板路径和字符串
        let templateStr = this.getSource(path.join(__dirname, 'main.ejs'));
        let code = ejs.render(templateStr, { entryId: this.entryId, modules: this.modules })
        // 资源（打包后的文件）
        this.assest = [];
        // 路径对应的代码
        this.assest[mian] = code;
        fs.writeFileSync(mian, this.assest[mian]);
    }

    run() {
        this.hooks.run.call();
        this.hooks.Compiler.call();
        // 执行并创建模块的依赖关系
        this.buildModule(path.resolve(this.root, this.entry), true);
        this.hooks.afterCompiler.call();
        // 发射打包后的文件
        this.emitFile()
        this.hooks.emit.call();
        this.hooks.done.call();
    }
}

module.exports = Compiler;
```

编译就由babel解决了

> babylon:将源码转成AST babel-tarvers:遍历节点 @babel/traverse babel-types:替换节点 @babel/types @babel/generator:生成代码

`npm i -S babylon @babel/traverse @babel/types @babel/generator`

[AST在线解析](https://astexplorer.net/)

![&#x53D6;&#x51FA;require&#x7684;&#x8FC7;&#x7A0B;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200923232219612.png)

![&#x8DEF;&#x5F84;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200923232736798.png)

### 编译模板

> **记得把注释删掉**

```javascript
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

## 手写loader

> ​ loader是一个函数，webpack会将需要解析代码作为参数传入
>
> 直接使用webpack的环境
>
> ​ 创建`loaders`文件夹

在webpack中添加配置

```javascript
    resolveLoader: {
        // 自动找
        modules: ['node_modules', path.resolve(__dirname, 'loaders')]
        // 别名获取loader
        // alias:{
        //     loader1: path.resolve(__dirname, 'loaders', 'loader1'),
        // }
    },
```

loader分类

`enforce` 配置 `'pre' 'normal' 'post'`

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

![pitch&amp;\#x6CA1;&amp;\#x6709;&amp;\#x8FD4;&amp;\#x56DE;&amp;\#x503C;&amp;\#x7684;&amp;\#x60C5;&amp;\#x51B5;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200925112736382.png)

#### pitchloader有返回值的情况

![pitchloader&amp;\#x6709;&amp;\#x8FD4;&amp;\#x56DE;&amp;\#x503C;&amp;\#x7684;&amp;\#x60C5;&amp;\#x51B5;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200925112856225.png)

### 实现babel-loader

```javascript
let babel = require("@babel/core");
const { RSA_NO_PADDING } = require("constants");
//loaderUtils 拿到预设  便于后期转化代码
let loaderUtils = require("loader-utils");
function loader(source) {
    // this是 loaderContext，包含大量loader信息
    let options = loaderUtils.getOptions(this)
    let cb = this.async(); // 包含异步函数
    babel.transform(source, {
        ...options,
        sourceMap: true,
        filename: this.resourcePath.split('/').pop(),
    }, function (err, result) {
        cb(err, result.code, result.map);// 异步
    })
}
module.exports = loader;
```

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

```text

```

### 实现style-loader

```javascript
function loader(source) {
    let style = `
    let style = document.createElement('style');
    style.innerHTML = ${JSON.stringify(source)}
    document.head.appendChild(style);
    `
    return style;
}

module.exports = loader;
```

### 实现less-loader

> 使用`less.render`函数将代码转化成css

```javascript
let less = require('less');
function loader(source) {
    let css = '';
    less.render(source, function (err, c) {
        css = c.css;
    });
    css = css.replace(/\n/g, '\\n')
    return css;
}

module.exports = loader
```

### 实现file-loader

```javascript
let loaderUtils = require('loader-utils')
function loader(source) {
    let filename = loaderUtils.interpolateName(
        this, '[hash].[ext]', { content: source }
    );
    this.emitFile(filename, source);
    return `modules.export=${filename}`
}

module.exports = loader;
```

### 实现banner-loader

```javascript
const loaderUtils = require('loader-utils');
const validate = require('schema-utils');// 校验配置参数
const fs = require('fs');
module.exports = function (source) {
    // this.cacheable(flase) // 不使用缓存，不建议，消耗内存大
    this.cacheable && this.cacheable(); // 只有第一次使用缓存，函数的默认值是true
    let options = loaderUtils.getOptions(this);
    let cb = this.async();
    let schema = {
        type: 'object',
        properties: {
            text: { type: 'string' },
            filename: { type: 'string' }
        }
    }
    validate(schema, options, 'banner-loader');
    // 给定模板
    if (options.filename) {
        this.addDependency(options.filename);// 添加依赖，这样就能被webpack watch到
        fs.readFile(options.filename, 'utf-8', function (err, data) {
            cb(err, `/**${data}**/${source}`)
        })
        // 只给定了文本
    } else {
        cb(null, `/**${options.text}**/${source}`)
    }
}
```

### 实现url-loader

```javascript
let loaderUtils = require('loader-utils')
const mime = require('mime');
function loader(source) {
    let { limit } = loaderUtils.getOptions(this);
    if (limit && limit > source.length) {
        return `modules.export=
        "data:${mime.getType(this.resourcePath)}
        ;base64,${source.toString('base64')}"`
    } else {
        return require('./file-loader').call(this,source)
    }
}
loader.raw = ture;
module.exports = loader;
```

## 手写插件

> 插件是一个类，里面一定包含一个函数apply
>
> ​ webpack会向钩子传入`compiler.hooks`，并自动执行
>
> ​ **注：**compiler是一个超大对象，可自行在控制台打印查看，里面包含大量可用的内容

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

### 手写将css插入到js的插件

> 减少http请求

```javascript
/**
 * @description 将HTML中的 link 直接变换成css代码 script 变成 js 以达到减少http请求的目的
 */
const HtmlWebpackPlugin = require('html-webpack-plugin');
class InlineSourcePlugin {
    /**
     *
     * @param {new RegExp()} param0
     */
    constructor({ match }) {
        this.reg = match;
    }
    /**
     *
     * @param {*} tag 标签对象
     * tagName: 'link',
     * voidTag: true,
     * attributes: { href: 'mian.css', rel: 'stylesheet' }
     *
     * tagName: 'script',
     * voidTag: false,
     * attributes: { defer: false, src: 'bundle.js' }
     * @param {*} compilation
     */
    processTag(tag, compilation) {
        let newTag, url;

        if (tag.tagName === 'link' && this.reg.test(tag.attributes.href)) {
            newTag = {
                tagName: 'style',
                attributes: { type: 'text/css' }
            }
            url = tag.attributes.href;
        }

        if (tag.tagName === 'script' && this.reg.test(tag.attributes.src)) {
            newTag = {
                tagName: 'script',
                attributes: { type: 'appliction/javascript' }
            }
            url = tag.attributes.src;
        }

        if (url) {
            // compilation编译对象  assets文件对象 source文件资源
            newTag.innerHTML = compilation.assets[url].source();
            // 删除js 和 css 文件
            delete compilation.assets[url];
            return newTag;
        }

        return tag;
    }

    // 处理所有标签
    processTags(data, compilation) {// 处理引入标签的数据
        let headTags = [];
        let bodyTags = [];
        data.headTags.forEach(headTag => {
            headTags.push(this.processTag(headTag, compilation));
        });
        data.bodyTags.forEach(bodyTag => {
            bodyTags.push(this.processTag(bodyTag, compilation));
        });
        return { ...data, headTags, bodyTags }
    }
    apply(compiler) {
        // 使用webpack实现功能
        compiler.hooks.compilation.tap('InlineSourcePlugin', (compilation) => {
            HtmlWebpackPlugin.getHooks(compilation).alterAssetTagGroups.tapAsync('alterPlugin', (data, cb) => {
                data = this.processTags(data, compilation); // compilation.assets
                cb(null, data)
            })
        })
    }
}

module.exports = InlineSourcePlugin;
```

