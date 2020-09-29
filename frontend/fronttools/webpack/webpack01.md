# webpack01

# 简介

## webpack功能

 代码转换：`(ES6、TypeScript…… 转化为 ES5 less=>css)`文件优化（压缩文件体积）、代码分割、 模块合并、自动编译和刷新页面、代码规范校验、自动发布

|                             |                                             |                                                              |                             |                      |                                                  |
| --------------------------- | ------------------------------------------- | ------------------------------------------------------------ | --------------------------- | -------------------- | ------------------------------------------------ |
| `entry`  **String\|Object** | ``                                          |                                                              |                             |                      |                                                  |
| `output` **Object**         | `filename`  **多文件[name].js**      `path` |                                                              |                             |                      |                                                  |
| `plugins` **Array**         | `new Plugin()`                              |                                                              |                             |                      |                                                  |
| `module` **Object**         | `rules`  **Array** `noParse` **RegExp**     | `test`  **RegExp**                               `use` **Array**                    `include` **String**                         `exclude` **RegExp\|String** | `loader` **String\|Object** | `options` **Object** | `presets` **Array**          `plugins` **Array** |
| `devServer` **Object**      |                                             |                                                              |                             |                      |                                                  |
| `mode` **String**           |                                             |                                                              |                             |                      |                                                  |
| `externals`  **Object**     | 不打包，直接引入CDN                         |                                                              |                             |                      |                                                  |
| `optimization`              |                                             |                                                              |                             |                      |                                                  |
| `devtool`                   |                                             |                                                              |                             |                      |                                                  |
| `watch`                     |                                             |                                                              |                             |                      |                                                  |
| `watchOptions`              |                                             |                                                              |                             |                      |                                                  |
| `resolve`                   |                                             |                                                              |                             |                      |                                                  |
| `resolveLoader`             | `modules` `alias`                           |                                                              |                             |                      |                                                  |


## 重要配置

### 四个核心概念

`entry`

`output`

`plugins`：

`loader` ：让webpack具有处理非**JavaScript**的能力

### 其他功能

`optimization` [提供优化配置](https://webpack.docschina.org/configuration/optimization/) 

`mode` 设置模式

`externals` 外部引入代码 （CDN的方式）

`module`配置loader

`devtools`  开启文件映射（打包后的代码错误难以定位）

```javascript
module:{
	rules: [{文件类型一},{文件类型二},{文件类型三}]
}
```

​	`rules` 中的可选配置

​			`test:/.less$/` 处理less后缀的文件

​			`use:`   `Array`

​						`ArrayItem:`    `Object|String` 对象可填入丰富的设置

​			`include:` 限定文件范围

​			`exclude:` 排除文件范围，通常排除 `/node_modules/`

​			`enforce:` 设置解析优先级，改变从下至上的默认顺序

[more](https://webpack.docschina.org/configuration/module/)

```javascript
 {
     test: require.resolve('jquery'),//引入了jquery时触发
     loader: 'expose-loader',
     options: {
         exposes: ['$', 'jQuery'],
     },
 },
```



## 执行方式

安装：

`npm i --save-dev webpack@4.44.1 webpack-cli` webpack脚手架

`npx webpack `  运行webpack

`npx webpack --config [自定义的配置文件名]` 读取自定义

### 自动刷新工具

1、安装`npm i webpack-dev-server`、启动：`npx webpack-dev-server`

 打包内存中而不是磁盘中，相关配置需要在`devServer`属性下设置

## 打包后的文件

> 以对象的形式，使用key value存储
>
> key 文件路径
>
> value function

```javascript
({
  "key1": (function () { eval("") }),
  "key2": (function () { eval("") }),
  ……
});
```

# loader

> webpack只支持解析`JS` 和 `json` 两种文件类型

`npm i less-loader style-loader --save`

```javascript
  module: {// 模块
        rules: [//规则
            // css-loader 处理@import语法
            //  style-loader 将css插入到head标签中
            {
                test: /\.css$/,
                use: [
                    {
                        loader: 'css-loader'
                    },
                    'style-loader']
            },
            // loader为了做到功能单一进行拆分
            // 从右向左执行
            // 用字符串或者对象（可以传入对象类型的options）存储到数组中
        ]
    }
```

## 处理css

提取css到一个单独的文件

`npm i mini-css-extract-plugin -D`

 `npm i optimize-css-assets-webpack-plugin -D`

 需要设置成生产环境：`production` 会自动**压缩成单行文件**

 `npm i terser-webpack-plugin -D`

css预处理：添加浏览器前缀

`npm i postcss-loader autoprefixer -D`

需要自行配置文件 `postcss.config.js` [npm介绍里面可以查看配置格式](https://www.npmjs.com/package/postcss-loader)

下面是我添加`webkit`的配置

```javascript
module.exports = {
    plugins: [
        require('autoprefixer')({
            'browsers':['last 5 version']
        })
    ]
}
```

## 处理js（babel）

> 将高版本语法转化为低版本语法，以提供更好的兼容性

es6`npm i -D babel-loader @babel/core @babel/preset-env`

@log 装饰器语法`npm i @babel/plugin-proposal-decorators --save` [官网文档](https://babeljs.io/docs/en/babel-plugin-proposal-decorators)

es7类中的直接赋值 `npm i @babel/plugin-proposal-class-properties --save`

```javascript
            {
                test:/\.js$/,
                use:{
                    loader:'babel-loader',
                    options:{//将es6转化为es5
                        presets:[
                            '@babel/preset-env'
                        ]
                    }
                }
            }
```

`npm install --save @babel/plugin-transform-runtime`

`npm install --save @babel/runtime`

[生成器函数、Promise语法](https://babeljs.io/docs/en/babel-plugin-transform-runtime#docsNav)

`npm i @babel/polyfill --save`

相当于是手写实现语法

如：includes

![&#x624B;&#x5199;&#x5B9E;&#x73B0;&#x7684;includes](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200825202144972.png)

### 代码规范（ESlint）

[配置文件获取](https://eslint.org/demo)

`npm i eslint-loader --save`

### 关于js引入

#### 全局loader

```javascript
            {
                test: require.resolve('jquery'),//引入了jquery时触发
                loader: 'expose-loader',
                options: {
                    exposes: ['$', 'jQuery'],
                },
            }
           import $ from 'jquery'
         console.log($);// object
    console.log(window.$);// object
```

#### 全局注入

> 给每个文件都提供

```javascript
        new webpack.ProvidePlugin({//提供插件
            $: 'jquery',
        })
         console.log($);// object
    console.log(window.$);// undefined
```

#### 引入不打包

> 以cdn形式引入

```javascript
    externals:{
        jquery:'$'//不打包$引入的内容，以cdn引入
    },
```

## 图片

`npm i -D file-loader`:内部生成一个文件到`output`目录下，并返回生成的图片名（hash名）

```javascript
            {
                test: /\.(png|gif|jpg)$/,
                use: 'file-loader',
            }
```

```javascript
import './index.less';
import gPic from './girl.jpg';//返回新的图片地址
console.log(gPic);//d05588a1b4148d177075c94437d9d5ec.jpg
let image = new Image();
image.src = gPic;
document.body.appendChild(image);
```

```css
body {
    div {
        width: 200px;
        height: 200px;
        border: 1px soild black;
        background-image: url("./girl.jpg");
    }
}
```

`npm i -D html-withimg-loader`从html中引入的方式因为文件名的改变导致无法读取，而这个loader解决了这个问题

```javascript
            {
                test: /\.html$/,
                use: "html-withimg-loader"
            }
```

更优

`npm i -D url-loader`\(更好的图片打包工具\)

> base64不用发送http请求，但base的图片会比原始图片大上1/3

```javascript
            {
                test: /\.(png|gif|jpg)$/,
                // 可以添加限制小于图片文件小于多少k时压缩成base64
                // 否则使用file-loader产生真正的图片
                use: {
                    loader: 'url-loader',
                    options: {
                        limit: 200 * 1024 //200k 当不想压缩是改为1即可
                    }
                }
            },
```

### 打包文件分离

### sourcemap（资源映射）

> 打包后的文件出现错误难以定位，sourcemap就是解决这个问题，`less、typescript`内都有

```javascript
 // devtool: 'source-map',//增加映射文件，包含行列信息（信息完整）
    // devtool: 'eval-source-map',//生成映射包含行列信息（信息完整），但不会出现单独的文件
    // devtool: 'cheap-module-source-map',// 不会产生列，增加映射文件，可以保留
    // devtool: 'cheap-module-eval-source-map',// 不会产生文件，集成再打包后的文件中，也不会产生列
```

## 解决跨域问题

```JavaScript
    // webpack-dev-Server 内置express模块
    devServer: {
        // 3) 有服务端 但不想用代理，前后端启动在一个端口

        // 2）mock数据
        // before(app) {
        //     app.get('/user', (req, res) => {
        //         res.json({ name: 'zoulam' })
        //     })
        // }

        // 1）
        // proxy:{//重写的方式 把请求代理到服务器上
        //     // 访问以api开头的就代理到后面这个url
        //     // '/api':'http://localhost:3000'
        //     '/api':{
        //         target:'http://localhost:3000',
        //         pathRewrite:{'/api':''},//把'/api'重写为空
        //     }
        // }
    },
```

`npm i -D webpack-dev-middleware`

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

### 设置依赖解析规则（ `resolve`）

```text

```

### 定义环境变量

### 区分环境变量

`npm i -D webpack-merge` 用于合并配置文件，区分生产和开发

```text

```

## 其他功能

### 懒加载

```javascript
let button = document.createElement('button');
button.innerHTML = 'click me'
button.addEventListener('click', function () {
    // console.log('lazy loading');
    // 草案中的语法,jsonp实现动态加载文件需要添加@babel/plugin-syntax-dynamic-import插件
    // 我在2020年使用的时候webpack已经支持了
    import('./source.js').then(data=>{
        console.log(data);
        console.log(data.default);
    })
})

document.body.appendChild(button);
```

### 热更新

> 热更新是不用重启服务器，有时候也被称为**增量更新**，在原来启动的服务器上重新部署

```javascript
    devServer: {
        hot:true,//启用热更新
        port: 3000,
        open: true,
        contentBase: './dist'
    },
        new webpack.NamedModulesPlugin(),//打印更新的路径模块
        new webpack.HotModuleReplacementPlugin(),//热更新插件
```

```javascript
import str from './source'
console.log(str);
if (module.hot) {
    module.hot.accept('./source', () => {
        // console.log('hot update');
        // import 只能在顶端使用
        let str = require('./source')
        console.log(str.default);
    })
}
```

