# less

> 1、less是一个css的增强版，通过less可以编写更少的代码实现更强大的样式
>
> 2、在less中添加了许多的新特性：像对变量的支持、对mixin的支持... ...
>
> 3、less的语法大体上和css语法一致，但是less中增添了许多对css的扩展， 所以浏览器无法直接执行less代码，兼容实现：**要执行必须向将less转换为css，然后再由浏览器执行**
>
> 4、支持嵌套，更符合程序员的DOM结构

## css的缺陷

**虽然CSS为了解决这些问题在新版本中提出的方案，但是旧的浏览器仍未实现兼容**

> 1、对于多出使用的`color`需要重复设置 `background-color` `color` `border`
>
> ​ 当需要更换网页风格时，每一处都可以修改
>
> ​ 对此可以使用变量
>
> ```css
> /*声明*/
> html{
>     --color:#bfa;
> }
> /*使用*/
> xx{
>     var(--color);
> }
> ```
>
> 2、

### `calc()`

> calc是一个计算函数，对于一些使用可以省去计算结果的麻烦，写入计算过程即可
>
> `calc(1000px/2)` 等价于 `500px`

## 语法简介

[官方文档](https://devdocs.io/less/)

安装插件`easy less`，一款在保存时将less转化为css的插件

当然也可以直接配置`Tasks.json`文件进行编译

### 嵌套

```css
body{
    background-color: yellowgreen;
    div{
        width: 200px;
        height: 200px;
    }
}
```

```css
body {
  background-color: yellowgreen;
}
body div {
  width: 200px;
  height: 200px;
}
```

### 变量

`//的注释不会被解析到CSS`

```css
//变量，在变量中可以存储一个任意的值
// 并且我们可以在需要时，任意的修改变量中的值
// 变量的语法： @变量名
@a:200px;
@b:#bfa;
@c:box6;

.box5{
    //使用变量是，如果是直接使用则以 @变量名 的形式使用即可
    width: @a;
    color:@b;
}

//作为类名，或者一部分值使用时必须以 @{变量名} 的形式使用
.@{c}{
    width: @a;
    background-image: url("@{c}/1.png");
}

// 同名变量会被覆盖，与JavaScript声明的变量类似

//引用
div{
    width: 300px;
    // 新版的语法
    height: $width;//引用上面设置的300px
}

// 数值可以直接运算
div{
    width:100px+200px;
}
```

### &符号

```css
.box1{
    .box2{
        color: red;
    }

    >.box3{
        color: red;

        &:hover{
            color: blue;
        }
    }

    //为box1设置一个hover
    //& 就表示外层的父元素
    &:hover{
        color: orange;
    }

    div &{
        width: 100px;
    }

    &-name{//名字的扩充

    }
}
```

### extend和mixin初览

```css
.p1{
    width: 100px;
    height: 200px;
}

//:extend() 对当前选择器扩展指定选择器的样式（选择器分组）
.p2:extend(.p1){
    color: red;
}


// 另类复制
.p3{
    //直接对指定的样式进行引用，这里就相当于将p1的样式在这里进行了复制
    //mixin 混合
    .p1();
}



// 专门被用做引用的，.p4()不会被直接解析到css中，需要被引用后才能解析
// 使用类选择器时可以在选择器后边添加一个括号，这时我们实际上就创建了一个mixins
.p4(){
    width: 100px;
    height: 100px;
}

.p5{
    .p4;
   // .p4();
}
```

### mixin

```css
//混合函数 在混合函数中可以直接设置变量
.test(@w:100px,@h:200px,@bg-color:red){//指定默认值
    width: @w;
    height: @h;
    border: 1px solid @bg-color;
}

div{
    //调用混合函数，按顺序传递参数
    // .test(200px,300px,#bfa);
    .test(300px);// 传参
    // .test(@bg-color:red, @h:100px, @w:300px);
}
```

### 内置函数

```css
//average
span{
   // font-size:average(100px,300px);//无效
    color: average(red,blue);//只能对颜色生效
}

html{
    width: 100%;
    height: 100%;
}
body {
    width: 100%;
    height: 100%;
    background-color: #bfa;
}

body:hover{
    background-color: darken(#bfa,50%);//对#bfa加深50%
}
```

[更多函数](https://devdocs.io/less/functions)

### 模块化

> 可以分成以下内容 **变量 动画效果 布局效果 整合模块**，方便后续的修改维护

```css
//import用来将其他的less引入到当前的less
//可以通过import来将其他的less引入到当前的less中
@import "syntax2.less";

.box1{
    // 在less中所有的数值都可以直接进行运算
    // + - * /
    width: 100px + 100px;
    height: 100px/2;
    background-color: #bfa;

}
```

## 调试

![&#x7F16;&#x8BD1;&#x914D;&#x7F6E;&#x53C2;&#x6570;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200731180102044.png)

![&#x914D;&#x7F6E;&#x6587;&#x4EF6;&#x5165;&#x53E3;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200731180255976.png)

```javascript
  "less.compile": {
    "compress": false, // 压缩,即将全部css文件写在一行，这样能够提升效率，但是浏览器会出现黄色警告
      //  true => remove surplus whitespace
    "sourceMap": true, // true => generate source maps (.css.map files)
    "out": true // 输出 false => DON'T output .css files (overridable per-file, see below)
  },
```

`sourceMap`生成调试文件，也就是编译文件的映射，`xxx.css.map`,这样就可以在开发工具里定位到`less`代码位置了

