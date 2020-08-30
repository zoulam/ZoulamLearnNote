# webpack01

## 简介

> webpack功能
>
> ​	代码转换：ES6、TypeScript…… =>  ES5  less=>css
>
> ​	文件优化：压缩文件体积
>
> ​	代码分割
>
> ​	模块合并
>
> ​	自动编译和刷新页面
>
> ​	代码规范校验
>
> ​	自动发布

### 重要配置

`entry`

`output`

`plugins`

`loader` ：让webpack具有处理非**JavaScript**的能力

### 执行方式

安装：

 `npm i --save-dev webpack@4.44.1 webpack-cli` 



`npx webpack`

`npx webpack --config [自定义的配置文件名]`



自动刷新工具

1、安装`npm i webpack-dev-server`、启动：`npx webpack-dev-server`

​	打包内存中而不是磁盘中，相关配置需要在`devServer`属性下设置

## 打包后的文件

> 以对象的形式，使用key value存储
>
> key 文件路径
>
> value function

```JavaScript
({
  "key1": (function () { eval("") }),
  "key2": (function () { eval("") }),
  ……
});
```

## loader

`npm i less-loader style-loader --save`

```js
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

`npm i mini-css-extract-plugin` 

​		`npm i optimize-css-assets-webpack-plugin`

​		需要设置成生产环境：`production`

​		`npm i terser-webpack-plugin`

​	

css预处理：添加前缀

`npm i postcss-loader autoprefixer` 

优化压缩css

## 处理js（babel）

es6`npm i -D babel-loader @babel/core @babel/preset-env`

@log`npm i @babel/plugin-proposal-decorators --save` [官网文档](https://babeljs.io/docs/en/babel-plugin-proposal-decorators)

（es7）类中的直接赋值 `npm i @babel/plugin-proposal-class-properties --save`

```JavaScript
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

相当于是手写实现

如：includes

![手写实现的includes](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200825202144972.png)



## 代码规范（ESlint）

[配置文件获取](https://eslint.org/demo)

`npm i eslint-loader --save`

## 关于js引入

### 全局loader

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

### 全局注入

> 给每个文件都提供

```javascript
        new webpack.ProvidePlugin({//提供插件
            $: 'jquery',
        })
         console.log($);// object
	console.log(window.$);// undefined
```

### 引入不打包

> 以cdn形式引入

```JavaScript
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

`npm i -D url-loader`(更好的图片打包工具)

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

## 打包文件分离

## sourcemap（资源映射）

> 打包后的文件出现错误难以定位，sourcemap就是解决这个问题，`less、typescript`内都有

```javascript
 // devtool: 'source-map',//增加映射文件，包含行列信息（信息完整）
    // devtool: 'eval-source-map',//生成映射包含行列信息（信息完整），但不会出现单独的文件
    // devtool: 'cheap-module-source-map',// 不会产生列，增加映射文件，可以保留
    // devtool: 'cheap-module-eval-source-map',// 不会产生文件，集成再打包后的文件中，也不会产生列
```

## 插件

> 

## 解决跨域问题

### 1、proxy

### 2、mock数据时

### 3、用服务端代码启动webpack（此时是同源）

`npm i -D webpack-dev-middleware`

```JavaScript
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

## 设置依赖解析规则（ `resolve`）

```

```

## 定义环境变量

## 区分环境变量

`npm i -D webpack-merge` 用于合并配置文件，区分生产和开发

```

```

# 优化

`npm i -D  webpack webpack-cli html-webpack-plugin @babel/core babel-loader @babel/preset-env @babel/preset-react`

@babel/preset-react解析jsx语法

## noparse

> 不去解析某些包，加快打包速度

```JavaScript
 module: {
        noParse:/jquery/, 
        }
```

## IgnorePlugin

[moment文档](https://momentjs.com/docs/)

> moment是一个解析时间的库，支持多语言，所以导致文件很大，webpack可以对其优化，即：只取出需要的语言包

```javascript
 module: {
 	rules: [
            {
                exclude: /node_modules/,
                include: path.resolve('src'),
                }]
                }
```

此处的例子中moment的指定语言版本需要手动引入

```javascript
import 'moment/locale/zh-cn';

moment.locale('zh-cn')
```



```javascript
const webpack = require('webpack');
 plugins: [
	new webpack.IgnorePlugin(/\.\/locale/,/moment$/), // 从moment中引入时，忽略./locale
]
```

## dllPlugin(动态链接库)

> **dynamic link library**
>
> react这个暂时不会变的库可以使用单独的配置文件打包，这样就不用每次都重新打包内容

```

```

## happypack

`npm i happypack -S`

> 启动多线程打包，**注：**小文件打包使用多线程反而会更慢，启动多线程也是需要花费系统资源的

```

```

## webpack自带优化

> 1、在生产模式下  `mode=production,`下使用`import`语法会自动去除没用的引入，
>
> 专业名词 `tree-shaking` ，树上有黄的和绿的叶子，光合作用低的黄页被认为是没用的，一摇就掉下来，故称**树摇优化**
>
> **注：**`require`语法是不支持`tree-shaking`
>
> 2、webpack会将已经引用且后续不再引用的代码整合
>
> 3、**抽离公共代码**，多入口中引用了重复代码自动剔除 
>
> ​	4.0以前的版本是使用插件 `commonChunkPlugins`实现

```javascript
 optimization: {
        splitChunks: {// 分割代码块
            //缓存组 ，存入频繁被引用的公共代码
            cacheGroups: {
                commons: {//公共模块
                    chunks: 'initial',//从入口开始找
                    minSize: 0,//最小是0byte
                    minChunks: 2,//最少引用次数之后抽离
                },
                vendor: {// 第三方
                    //提高权重，先抽离第三方模块，再抽离上面指定的模块
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

# 其他功能

## 懒加载

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

## 热更新

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

