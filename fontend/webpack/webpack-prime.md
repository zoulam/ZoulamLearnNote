# \[webpack\]使用

## webpack

```javascript
命令行
    运行指定配置文件 npx webpack --config <configFileName>
    也可以进行覆盖操作(对无法修改的配置文件进行简单修改)
        npx webpack --mode 更换模式
        npx webpack --hot/-h 启动热更新
        npx webpack --progress
webpack原理
    读取webpack.config.js导出的js对象进行打包    
    实现了名为 __webpackrequire__的函数
        以 key:value 的形式取加载各个file，通过eval封装

包的安装，一般有关于打包的能力都是开发依赖所以是devDependencies

四个重要概念
    entry（入口）
    output（出口）

    loader（函数，处理非js成js）
        读取顺序（没有设置优先级） 右->左 下=>上
                less-loader(编译成css)
             => css-loader（转化为js嵌入样式，解析url()为require()）                          => style-loader
        能力：（面向代码文件）语法转换、向指定的js靠拢

    plugin（类/构造函数，添加webpack处理js的能力）
        能力:（面向webpack）打包优化、文件管理、环境注入。

优化思路
    1、优化打包速度
    2、优化开发的编译速度
    3、优化static文件体积
    4、优化服务器配置

其他配置
    devServer（开发配置）
        包括代理（跨域）、
        mock数据

概念
    开发模式vs生产模式
        (可以写两个配置文件，配合npm script 使用，更懒的方式是使用合并公共配置的方式)
    development： 
        代码存在于内存中、
        不会压缩代码（会换行）、
        有sourceMap、
        会调用devDependencies包和dependencies包
    production：只调用dependencies的包、压缩成一行
                出现静态文件
```

[webpack官网](https://www.webpackjs.com/)

> 本文是基于webpac4.0的笔记，webpack是使用node写的js代码，实现主要依赖[tapable](https://www.npmjs.com/package/tapable)的发布订阅的事件流模式。

## 常用配置

安装使用

```text
npm init -y
cnpm install webpack webpack-cli -D
npx webpack 【运行webpack.js，再通过这里的代码运行webpack-cli】
npx webapck --config wbepack.common.js 【执行自定义配置文件，搭配npm-script食用】

【npm 脚本传参】假设脚本是
'build':'webpack'
 npm run build -- --config wbepack.common.js

node_modules\webpack-cli\bin\utils\convert-argv.js在这里可以查看默认配置文件名：
    webpack.config.js webpackfile.js
```

```javascript
module.export = {
    optimization:(Object)
    mode:(String)
    entry:(String | Object) Object是多页应用使用的
    output:(Object)
    plugins:(Array)
    module:(Object)
    devServer:(Object)
    externals:(Object)
    devtool:(String)
    watch:(Boolean) true就是监控代码变化【以保存为准】实时打包
    watchOptions:(Object)
    resolve:(Object) 强制使用哪个路径的包
}
```

### 1、mode

> tree-shaking：引用的包中没有使用的部分自动删除
>
> **注：** import 语法支持 【导出使用的部分，编译时加载】
>
> require语法【导出对象全部内容，运行时加载】不支持

```javascript
// 其他代码需要自己配置
mode:"development" 不压缩js代码,不进行 tree-shaking 
mode:"production" 压缩js代码（一行）,进行 tree-shaking
```

### 2、enrty

```text
"./src/index.js"//入口文件路径
```

### 3、output

```javascript
{
    // .[hash:8] 每次打包的hash都不一样,":8"是指hash的长度为8，即一次打包就生成一个新的js文件
    filename:'bundle.[hash:8].js', // 打包后的文件名,可以选择添加 
    path:path.resolve(__dirname,'dist'),// 输出路径
    publicPath:'http://www.zoulam.org',// 给静态文件【包括js文件，所以不建议使用】添加域名
}
```

### 原理入门

```javascript
实现了 __webpack_require__ 函数
将模块放入只执行函数
(function(modules){})(
    {
        // path 是字符串路径，code是函数
        <path1>:<code1>,
        <path2>:<code2>
    }
)

执行流程
__webpack_require__(入口文件路径){
    1、是否缓存
    2、modules[入口文件路径],取出代码执行代码，module.export 被替换成__webpack_require__ 
    3、执行直到完成
}
```

### 4、devServer

```text
npm install webpack-dev-server -D
```

```text
直接使用,不会生成磁盘文件，而是生成再内存中
npx webpack-dev-server
```

```javascript
devServer: {// 开发服务器配置
    port: 3000,
    progress: true,// 显示编译进度，在浏览器控制台中可以看到
    contentBase: './build',// 文件夹
    compress: true,// 是否压缩
    open: true, // 自动打开浏览器
},
```

### [5、devtool](https://webpack.js.org/configuration/devtool/#root)

常用的四个

```text
// 不生成单独文件直接注入到代码中，生成映射包含行列信息（信息完整）
'cheap【只有行信息】-module-source-map'// 只定位到行，增加映射文件
'cheap-module-eval-source-map'// 不会产生文件，集成再打包后的文件中，也不会产生列
```

![source&#x672A;&#x6CE8;&#x5165;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201104182443512.png)

![source&#x6CE8;&#x5165;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201104182522774.png)

![&#x7B80;&#x5355;&#x6620;&#x5C04;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201104182754270.png)

![source&#x6CE8;&#x5165;&#x548C;&#x7B80;&#x5355;&#x6620;&#x5C04;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201104183532437.png)

|  |  |
| :--- | :--- |
| `‘source-map’` | 生成\[`xx.js.map]`文件，错误定位到准确的行列【上图一】 |
| `'eval-source-map'` | 无source文件，错误只定位到**行**【上图二】 |
| `'cheap-module-source-map'` | 生成\[`xx.js.map]`文件，打包结果【上图三】文件内容见下面，与源码无关联【即无法定位】 |
| `'cheap-module-eval-source-map'` | 无source文件，错误只定位到**行**【上图四】 |

```text
{"version":3,"file":"home.js","sources":["webpack:///home.js"],"mappings":"AAAA","sourceRoot":""}
```

### 6、plugins

> 插件使用与先后顺序有关

```javascript
cnpm install html-webpack-plugin -D // html模板
cnpm installl mini-css-extract-plugin -D // 提取css到指定文件而不是直接插入到html文件中
// 注：需要将style-loader改为
    MiniCssExtactPlugin.loader
// 不然依旧会被插入到html中
cnpm postcss-loader autoprefixer -D// 自动添加前缀的插件，需要搭配postcss-loader使用
```

```javascript
const HtmlWebPlugin = require('html-webpack-plugin')
const MiniCssExtactPlugin = require('mini-css-extract-plugin')

plugins:[
        new HtmlWebPlugin({
        template: './src/index.html',// 模板
        filename: 'index.html',// 输出的文件名
        minify: {// 压缩方案
            removeAttributeQuotes: true,// 删除html中的双引号
            collapseWhitespace: true,// 压缩成单行
        },
        hash: true,// 生成给引入的JavaScript文件名生成hash戳
    }),
    // 提取html内的css
    new MiniCssExtactPlugin({
        filename: 'css/mian.css'// 在打包文件夹下的css文件夹下的mian.css
    }),
]
```

#### 小插件

```bash
npm install cleanWebpackPlugin -D
1、cleanWebpackPlugin // 清除上一次打包的文件
npm install copy-webpack-plugin -D
2、copyWebpackPlugin // 拷贝文件，webpack打包一起,一些文件只要拷贝而不需要打包
3、bannerPlugin // 添加声明如：代码作者信息等【webpack内置】
```

```javascript
const { CleanWebpackPlugin } = require('clean-webpack-plugin')
const copyWebpackPlugin = require('copy-webpack-plugin')
const webpack = require('webpack')
plugins: [
    new CleanWebpackPlugin(),// 默认删除output文件夹下的内容，也可以填入数组或者字符串删除指定文件
    new copyWebpackPlugin({
        patterns: [
             { from: 'doc', to: './' }// 将doc下的文件拷贝到dist
         ]
    }),
    new webpack.BannerPlugin('create 2020/8 by zoulam')
]
```

#### 环境变量

```javascript
plugins: [
    new webpack.DefinePlugin({
        // 下面大写的key名字是自定义的，使用时保持一致即可
        // DEV:"'dev'", // 默认解析成变量名而不是字符串，需要包裹一层
        DEV:JSON.stringify('dev'), // 这样就是字符串了
        FLAG:'true',
        EXPRESSION:'1+1'
    }),
]
```

```javascript
let url = '';
if(DEV){
    url='htttp://localhost:3000'
}else{
    url='http://www.luluxi.com'
}
console.log(url);
console.log(typeof FLAG);// boolean
console.log(typeof EXPRESSION);// 2
```

### 7、optimization

> 生产环境才会调用这里的参数

```javascript
cnpm install optimize-css-assets-webpack-plugin -D // 压缩css代码为1行
npm install uglifyjs-webpack-plugin -D //压缩js代码为1行
cnpm install terser-webpack-plugin -D // 压缩js代码为1行
```

```javascript
const optimizeCSS = require('optimize-css-assets-webpack-plugin')
const TerserJSPlugin = require('terser-webpack-plugin')
const UglifyJsPlugin = require('uglifyjs-webpack-plugin')
optimization: {// 优化项
    minimizer: [
        new UglifyJsPlugin({// 压缩js为1行
            cache: true,// 使用缓存
            parallel: true,// 多文件压缩
            sourceMap: true,// 生成映射
        }),
        // new optimizeCSS(), // 压缩css代码为1行
        new TerserJSPlugin({}),// 压缩js代码为1行
    ],
},
```

### 8、module **\(loader\)**

> loader是从右往左，从下往上读取
>
> less-loader =&gt; post-loader =&gt; css-loader =&gt; style-loader
>
> loader 有四种
>
> prev-loader（前置loader）
>
> normal-loader
>
> post-loader
>
> 内联loader： 【如：expose-loader】，可以直接卸载代码块中的loader
>
> ```javascript
>  import $ from 'expose-loader?$!Jquery' // 将Jquery以$暴露出去
>  console.log(window.$)
> ```

```text
module:{
    rules:[ // 数组类型的规则
        test:(RegExp)
        use:(Array | String) 【数组元素可以是string | Object】
        includes:(String | RegExp) path.join(__dirname, 'index.js')
        excludes:(String | RegExp) /node_modules/
        enforce:(String) prev normal(默认) post
    ]
}
```

```text
cnpm install css-loader style-loader -D
sass-loader【编译sass成css】
css-loader【支持@import语法，并且将background:url('path')转化为background:url(require("path"))】 
postcss-loader 【添加前缀】需要添加配置 postcss.config.js ,或者直接写入 webpack配置中
style-loader【将css注入到DOM】
```

#### loader为Object

```javascript
module：{
    rules:[ // 数组类型的规则
        test:/\.css$/
        use:[
        {
            loader: "style-loader", 
            options: { // options 可以写成单独的配置文件
                insert: 'top'
            }
        },
            'css-loader'
        ]
    ]
}
```

#### [post-loader](https://www.npmjs.com/package/postcss-loader)

```javascript
// postcss.config.js
module.exports = {
    plugins: [
        require('autoprefixer')({
            'browsers':['last 5 version']
        })
    ]
}
```

#### babel-loader

[decorators语法](https://babeljs.io/docs/en/babel-plugin-proposal-decorators)

```javascript
cnpm install babel-loader @babel/core @babel/preset-env -D
babel-loader【loader】 @babel/core【核心模块】 @babel/preset-env【es6=>es5】 -D

-----------------------------------------------------------------------------------
cnpm install @babel/plugin-proposal【提案】-class-properties -D
支持下面的语法
class A{
    a = 1;
}
let a = new A()
console.log(a.a)// 1

-----------------------------------------------------------------------------------
cnpm install @babel/plugin-proposal-decorators -D // 装饰器语法
@foo
class A{}

function foo(target){
    console.log(target) // class A{}
}

-----------------------------------------------------------------------------------
npm install @babel/plugin-transform-runtime -D // 解析运行时的语法，如生成器函数同时抽离公共代码段
npm install @babel/runtime -S
// 支持新的api 如 es7 的 String.prototype.includes(),会用老语法手写一个
npm install @babel/polyfill -S 

require('@babel/polyfill')
let str = 'aa'
str.includes('a')
```

![includes](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200825202144972.png)

```javascript
module：{
    rules:[ // 数组类型的规则
        test:/\.js$/
        use:[
        {
            loader: "babel-loader", 
            options: {
                presets:['@babel/preset-env'],
                plugins: [
                    ["@babel/plugin-proposal-decorators", { "legacy": true }],
                    ["@babel/plugin-proposal-class-properties", { "loose": true }],
                    "@babel/plugin-transform-runtime"
                ]
            }
        }]
    ]
}
```

#### [eslint](https://eslint.org)

```text
npm install eslint  eslint-loader -D
```

写配置文件（.eslintrc.json）图形化界面选项[eslint](https://eslint.org/demo)

```javascript

```

```javascript
{
    test:/\.js$/,
    use:{
        loader:'eslint-loader',
    },
    enforce:'pre', //pre强制提前执行 默认值是normal post滞后执行
    exclude:/node_modules/
},
```

#### 全局变量挂载

> expose-loader挂载在window上
>
> webpack.ProvidePlugin注入到文件中
>
> externals剔除cdn引入的

```bash
cnpm install Jquery -S
```

```javascript
import $ from 'Jquery'
console.log(window.$) // undefined
```

```text
npm install expose-loader -S
```

```javascript
// 旧写法 --> 报错：options misses the property 'expose'
// import $ from 'expose-loader?$!jquery';// 将Jquery以$暴露出去
// 新写法
import $ from 'expose-loader?exposes=$!jquery';// 将Jquery以$暴露出去
console.log(window.$) // 正常输出
```

写入配置而不是用内联loader

```javascript
{
    test: require.resolve('jquery'),//引入了jquery时触发
    loader: 'expose-loader?$',
}

import $ from 'Jquery' // 还是需要引入


---------------------或者直接注入-------------------------------- 
new webpack.ProvidePlugin({//提供插件
    $: 'jquery',// 每个模块都注入 $
}),    

{
    test: require.resolve('jquery'),//引入了jquery时触发
    loader: 'expose-loader',
    options: {
        exposes: ['$', 'jQuery'],
    }
}
```

#### externals

```javascript
externals:{
    jquery:'$'//不打包$引入的内容，以cdn引入
},
```

#### 图片loader

> 引入方式
>
> 1、 js引入 【file-loader支持】
>
> 2、css的`background:url("path")` 【css-loader支持】
>
> 3、`<img src="url" />`【html-withimg-loader支持】

```javascript
// 需要先引入再使用，不然被认为是普通字符串
// const pic = require('./pic.png')
import pic from './pic.png'
let img = new Image()
img.src = pic
document.body.appendChild(image)
```

```text
cnpm install file-loader -D // 默认在内部生成文件到打包后的路径下并引入
```

```javascript
{
    test:/\.(png|gif|jpg)$/,
    use:{
        loader:'file-loader',
    }
},
```

```text
cnpm install html-withimg-loader -D
```

```javascript
{
    test: /\.html$/,
    use: "html-withimg-loader"
}
```

**base64支持**

> base64 文件更大（1/3），但是不用发送base64http请求

```text
cnpm install url-loader -D
```

```javascript
{
    test: /\.(png|gif|jpg)$/,
    // 可以添加限制小于图片文件小于多少k时压缩成base64
    // 否则使用file-loader产生真正的图片
    use: {
        loader: 'url-loader',
        options: {
            limit: 200 * 1024,// 超过200k就是文件，否则是base64
            outputPath:'/img/',// 指定输出路径
            publicPath:'http://localhost:8080'，// 值给图片添加cdn前缀
        }
    }
}
```

### 多页面配置

```javascript
const htmlWebpackPlugin = require('html-webpack-plugin');
module.exports ={
    entry: {
        home: './src/index.js',//key值可以随便起名，常用mian和home作为主入口
        other: './src/other.js'
    },
    output: {
        // [name]依次读取上面的home和other
        filename: '[name].js',
        path: path.resolve(__dirname, 'dist')
    },
    plugins: [
    new htmlWebpackPlugin({
        template: './src/index.html',
        filename: 'home.html',
        chunks: ['home'],// 只引入home.js的文件
    }),
    new htmlWebpackPlugin({
        template: './src/index.html',
        filename: 'other.html',
        chunks: ['home', 'other'],// 引入home.js和other.js的文件
    })
],
}
```

### 9、watchOptions

```javascript
watchOptions: {
    poll: 2000,// 监控间隔事件单位ms
    aggregateTimeout: 500,// 防抖 输入完成xxms后才打包
    ignored:/node_modules/, // 忽略文件
},
```

### webpack跨域**\(devServer\)**

> webpack内置`express`,不用安装就可以引入
>
> 前端：`http://localhost:8080`
>
> 后端：`http://localhost:3000`

#### 有服务端http-proxy

**后端**

```javascript
let express = require('express');
let app = express();

app.get('/user', (req, res) => {
    res.json({ name: 'zoulam' })
})

app.listen(3000);
```

**前端**

```javascript
let xhr = new XMLHttpRequest();
// 使用http-proxy来实现代理
xhr.open('GET', 'api/user', true); // true 是异步

xhr.onload = function () {
    console.log(xhr.response);
}

xhr.send();
```

**webpack配置**

```javascript
devServer:{
    // http-proxy
    proxy:{
        './api':{
             target:'http://localhost:3000',
             pathRewrite:{'/api':''},//把'/api'重写为空 这样就能直接找到user了
        }
    }
}
```

#### 无服务器mock数据

```javascript
devServer:{
    before(app) {
        app.get('/api/user', (req, res) => {
            res.json({ name: 'zoulam' })
        })
    }
}
```

#### 有服务端同源处理

> 在服务端启动webpack，前后端在同一个端口，**无需配置devServer**

```text
npm install webpack-dev-middleware -D
```

```javascript
let express = require('express');
let app = express();

let webpack = require('webpack')
const middle = require('webpack-dev-middleware');

let config = require('./webpack.config.js');

let compiler = webpack(config);

app.use(middle(compiler));

app.get('/user', (req, res) => {
    res.json({ name: 'zoulam' })
})

app.listen(3000);
```

### resolve

> package.json 的 main字段是默认入口

#### 重置入口

```text
cnpm install bootstrap -S
```

```javascript
// 会去node_modules下的bootstrap下的package.json找main字段的入口
// import from 'bootstrap' // 引入的实际是js
import from 'bootstrap/dist/css/bootstrap.css'
```

```javascript
------------------------方法1-----------------------------------
resolve:{
    modules: [path.resolve('node_modules')],
    // 修改主入口 默认是是main （在package.json内）
    mainFields: ['style', 'main'],// 先找style参数，找不到再去找main入口
    // 修改入口文件名 默认是index.js
    mainFiles: [], 
}
------------------------方法2-----------------------------------
resolve:{
    modules: [path.resolve('node_modules')],
    alias:{
        bootstrap: 'bootstrap/dist/css/bootstrap.css'
    }    
}
```

```text
import from 'bootstrap'
```

#### 添加后缀

> webpack **在默认情况下** 只能省略js类型的后缀，其他省略找不到。

```javascript
-----------------index.css引入忘记添加后缀-------------
import from 'index'
```

```javascript
resolve: {
    modules: [path.resolve('node_modules')],
    // 添加默认扩展名
    extensions:['.js','.css','.json','vue'],// 从左到右，逐个后缀名添加尝试
},
```

### 开发和生产配置

```text
webpack.base.js // 公共的基础配置
webpack.dev.js // 开发配置
    源码映射 开发mode
webpack.prod.js // 生产配置
    优化项 生产mode
```

合并配置

> 类似`Object.asign()`的能力，后面的同名会覆盖前面的

```text
npm install webpack-merge -D
```

```javascript
let { merge } = require('webpack-merge')
let base = require('./webpack.base.js')

module.exports = merge(base, {
    mode: 'development'
})
```

## webpack优化

### 1、noParse

**加快打包速度**

```javascript
module:{
    noParse:/jquery/, // 不去解析该库【jquery】的依赖项，前提是没有
}
```

### 2、排除和包含

减少查找

```javascript
module:{
    exclude: /node_modules/,
    include: path.resolve('src'),
}
```

### 3、IgnorePlugin

> moment是一个解析时间的库，支持多语言，【**场景是只需要中文**】

```text
cnpm install moment -S
```

```javascript
import jquery from 'jquery';
import moment from 'moment';

moment.locale('zh-cn')
let r = moment().format("dddd, MMMM Do YYYY, h:mm:ss a");
console.log(r);
```

```javascript
const webpack = require('webpack');

plugins: [
     new webpack.IgnorePlugin(/\.\/locale/, /moment$/), // 从moment中引入时，忽略./locale
]
```

忽略后需要手动引入

```javascript
import 'moment/locale/zh-cn';
```

### 4、dll

> `dynamic link library` 动态链接库【**直接打包一次，打包成变量，后面直接引用就可以了**】
>
> 场景：暂时不准备更新的库，如：react稳定版，只要是第三方库都适用这个方法

```javascript
import React from 'react';
import {render} from 'react-dom';

// render(<h1>hello jsx</h1>, window.root) // window.root直接获取id为root的节点
render(<h1>hello jsx</h1>, document.getElementById('root'))
```

单独开一个webpack.react.js

```javascript
const path = require('path');
const webpack = require('webpack');
module.exports = {
    mode: 'development',
    entry: {
        // test: './src/test.js',
        react: ['react', 'react-dom'],//打包这两个包
    },
    output: {
        filename: '_dll_[name].js',
        path: path.resolve(__dirname, 'dist'),
        // library: 'reactPackage',
        library: '_dll_[name]', // 打包后生产的变量
        // libraryTarget:'commonjs', // 默认值是：var， commonjs是export
        // libraryTarget:'umd',// 统一资源模块
    },
    plugins: [
        new webpack.DllPlugin({
            name:'_dll_[name]',// name === library
            // 保存打包后的引用关系,mainfest.json常被称为任务清单
            path:path.resolve(__dirname,'dist','mainfest.json'),
        })
    ]
}
```

```text
npx webpack --config webpack.react.js
```

> 输出两个文件 `_dll_react` （动态链接库）和 `mainfest.json`\(引用关系\)

```javascript
---------------------------html模板中------------------------------------
<script src='_dll_react.js'></script>
```

**正式的配置文件**

> 先找动态连接库，没有就找 `node_modules`

```javascript
plugins: [
    new webpack.DllReferencePlugin({//优先去指定文件下引入，没有才取打包
        manifest: path.resolve(__dirname, 'dist', 'mainfest.json')
    }),
]
```

### 5、多线程打包

> 计算线程分配需要花费性能，小文件用多线程更慢

```text
npm install happypack -D
```

```javascript
const Happypack = require('happypack');

module:{
    rules:[
       {
            exclude: /node_modules/,
            include: path.resolve('src'),
            test: /\.js$/,
            // 使用Happypack下的loader模块，并指定打包的是js ?id=js
            use: 'Happypack/loader?id=js',
        },
        {
            test:/\.css$/,
            // 使用Happypack下的loader模块，并指定打包的是css ?id=css
            use:'happypack/loader?id=css'
         }
    ]
},
plugins: [      
     new Happypack({
            id: 'css',// 对应上面的id
            use: ['style-loader','css-loader'],
        }),
        new Happypack({
            id: 'js',
            use: [{
                loader: 'babel-loader',
                options: {
                    presets: [
                        '@babel/preset-env',
                        '@babel/preset-react'
                    ]
                }
            }],
        })
]
```

### 6、webpack原生优化

> **原生优化指不用做任何配置的优化**
>
> tree shaking 和 scope hosting

```javascript
----------------------文件1（test.js）-------------------------------
let sum = function (a, b) {
    return a + b + 'sum';
}

let minus = function (a, b) {
    return a - b + 'minus';
}

export default {
    sum,
    minus
}
----------------------文件2-------------------------------
import calc from './test'
console.log(calc.minus(1, 2));
// const calc = reuqire('./test') // 不支持tree-shaking
// console.log(calc.default.minus(1, 2));
```

```javascript
-------------------------编译前------------------------------
// scope hositing
let a = 1;
let b = 2;
let c = 3;
let d = a + b + c;
// webpack 3.0之后会自动省略声明后并且被使用的变量（即直接输出6）
console.log(d);
-------------------------生产模式编译后-----------------------
console.log(6)
```

### 7、抽离公共代码

> 场景：多入口多出口文件
>
> 都引入了 a和b模块

```javascript
--------------------index.js--------------------------
import './a'
import './b'
console.log('index.js');

import $ from 'jquery'
console.log($);

--------------------other.js--------------------------
import './a'
import './b'
console.log('index.js');

import $ from 'jquery'
console.log($);
```

```javascript
 optimization: {
    splitChunks: {// 分割代码块
        // 缓存组 ，存入频繁被引用的公共代码
        cacheGroups: {
            commons: {//公共模块
                chunks: 'initial',//从入口开始找
                minSize: 0,//最小是0byte
                minChunks: 2,//最少引用次数之后抽离
            },
            vendor: {// 第三方 【jquery】
                // 提高权重，先抽离第三方模块，再抽离上面指定的模块，不然第三方模块会被加入上面缓存组
                priority: 1,
                // 引入过node_modules，就将他分离
                test: /node_modules/,
                chunks: 'initial',
                minSize: 0,
                minChunks: 2,
            }
        }
    }
},
```

### 8、懒加载

> 草案中的语法,`jsonp`实现动态加载文件需要添加`@babel/plugin-syntax-dynamic-import`插件 我在2020年使用的时候webpack已经支持了
>
> **vue和react的懒加载都是这样实现的**

```text
let button = document.createElement('button');
button.innerHTML = 'click me'
button.addEventListener('click', function () {
    import('./source.js').then(data=>{
        console.log(data);
        console.log(data.default);
    })
})

document.body.appendChild(button);
```

### 9、热更新

> 在不重启服务器的情况下更新代码内容，也叫增量更新

```javascript
devServer: {
    hot:true,//启用热更新
    port: 3000,
    open: true,
    contentBase: './dist'
},
plugins: [
    new webpack.NamedModulesPlugin(), // 打印更新的路径模块
    new webpack.HotModuleReplacementPlugin(),// 热更新插件
]
```

```javascript
---------------------source.js----------------------------------
export default 'luluxi56'


---------------------index.js----------------------------------
import str from './source'
console.log(str);
if (module.hot) {
    // 监听./source文件，更新了就启动热更新
    module.hot.accept('./source', () => {
        // console.log('hot update');
        // import 只能在顶端使用
        let str = require('./source')
        console.log(str.default);
    })
}
```

### 10、[more](https://juejin.im/post/6844903651291447309#heading-1)

