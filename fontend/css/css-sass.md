# \[css\]scss

> ​	基于ruby语法开发是css处理器，现在有两种编译包在npm上，一种是 `dart-scss` （性能更好，更新更快），`node-sass`（不推荐使用）。
>
> ​	使用更加灵活的的语法写css减少代码量，但是最终还是需要编译为css才能使用。\
>
> ​	[官网](https://sass-lang.com/)
>
> ​	[在线编译平台](https://www.sassmeister.com/)
>
> ​	[你可能不知道的 Sass 技巧](https://medium.com/d-d-mag/%E4%BD%A0%E5%8F%AF%E8%83%BD%E4%B8%8D%E7%9F%A5%E9%81%93%E7%9A%84-sass-%E6%8A%80%E5%B7%A7-c97d4d5e0fc4)

## 注释方式

```css
/**/
//
```

## scss和sass的区别

```css
// 有大括号和分号
// SCSS SYNTAX
$font-stack:    Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}


// 没有大括号和分号
SASS SYNTAX
$font-stack:    Helvetica, sans-serif
$primary-color: #333

body
  font: 100% $font-stack
  color: $primary-colo
```

## react环境

```bash
cnpm install node-sass -D # cra就不用配置，自己搭建需要配置scssloader规则
```

## 自行搭建环境

```
npm install css-loader style-loader node-sass sass-loader  --save-dev
```

```JavaScript
const path = require("path");
const {VueLoaderPlugin} = require('vue-loader');
module.exports = {
    entry: './webapp/App.js',
    output: {
        filename: 'App.js',
        path: path.resolve(__dirname, './dist')
    },
	module: {
		rules: [
            {
                test: /\.scss/,
                use: ['style-loader', 'css-loader','sass-loader']
            },
			{
				test: /\.vue$/,
				use: 'vue-loader'
			}
		]
	},
	plugins: [
		new VueLoaderPlugin()
	],
	mode: "production"
}
```



## 文件组织（模块化）

```javascript
// 全局样式
# 直接引入到 App.js的样式 这一文件是引入其他scss文件的入口，引入是省略下划线
import "./styles/index.scss"

styles/index.scss 

// index文件内容：
@import "variables"; // 不用./
@import "../components/Button/style"; // 需要返回上级目录

# 也是自定义mixin的一种不过单独划分了文件
styles/_animation.scss
# 自定义mixin
styles/_mixin.scss
# scss中去除浏览器默认样式并做出美化，实现各个浏览器的表现是一致如：设置border-box为content-box
styles/_reboot.scss 
# 存放大量的变量
styles/_variables.scss

// 局部样式
Components/Component1/_style.scss
Components/Component2/_style.scss
```

## scss规则

### 1、变量声明，和default

```css
$<variabelName> : <cssVariable> !default;
$color: #000;
$color: #fff !default;
div{
    color:$color; // 不会被覆盖#000
}    

// 变量使用和拼接
$side : left;
#main{
    color: $warning;
	margin-#{$side}:10px;
}

```

### 2、选择器

```css
#main{
    color: red;
    &box{}// #main box,&可以省略
    >box2{} # 父子
    +box3{} # 兄弟
}
```

### 3、计算

>  支持加减乘除，`+  -  *  /  %`

```css
.div2 {
    margin: 10px * 2;
    padding:(14px / 2);
}
```

### 4、属性嵌套

```css
p{
    border: {  //注意，border后面必须加上冒号。
        color: #000; //  border-color: #000;
        style: none;
    }
}
```

### 5、继承

```css
.div3 {
    margin: 2px;
}

/* .div4继承.div3 */
.div4 {  
    @extend .div3;
    font-size: 10px;
}

```

编译后

```css
.div3, .div4 {
  margin: 2px;
}

/* .div4继承.div3 */
.div4 {
  font-size: 10px;
}
```

### 6、自定义mixin

>  生命@mixin，使用@include

```css
@mixin p1 {
    float: left;
}

div {
/*使用@include命令，调用这个mixin。*/
    @include p1; 
    top: 10px;
}
```

编译后

```css
div {
  /*使用@include命令，调用这个mixin。*/
  float: left;
  top: 10px;
}
```

填入参数

```css
@mixin box-sizing ($sizing) {
    -webkit-box-sizing:$sizing;     
       -moz-box-sizing:$sizing;
            box-sizing:$sizing;
}
.box-border{
    border:1px solid #ccc;
    @include box-sizing(border-box);/*引用*/
}
```

填入默认值

```css
/*默认值*/
@mixin p2($val1, $val2:20px) {
/* 如果不加入参数，就用默认参数 */
    float: $val1;
    top: $val2;
}
//使用的时候，根据需要加入参数：
div {
    @include p2(left);
}
```

### 7、流程控制

```css
// 流程控制
@mixin test($condition) { 
  $color: if($condition, blue, red); 
  color:$color 
}
  
.firstClass { 
  @include test(true); 
} 
 
.secondClass { 
  @include test(false); 
}

@mixin txt($weight) { 
  color: white; 
  @if $weight == bold { font-weight: bold;} 
} 

.txt1 { 
  @include txt(none); 
} 

.txt2 { 
  @include txt(bold); 
}
```

### 8、循环

```css
// for循环
@for $i from 1 through 4 { 
  .col-#{$i} { 
    width:#{$i} * 25px  ;
  } 
}
```

### 9、数据结构

>  map list

```css
// map创建
$primary:     red !default;
$theme-colors:
(
  "primary":    $primary,
)

// map使用，当元素的类名是： icon-primary 就会变成红色
@each $key, $val in $theme-colors {
  .icon-#{$key} {
    color: $val;
  }
}
```

## 常见操作

### 强制全局

```text
scss存在块级作用域，但可以使用!global将变量提升到全局
```







