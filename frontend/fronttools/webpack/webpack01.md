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

