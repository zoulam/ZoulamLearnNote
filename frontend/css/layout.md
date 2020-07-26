# 常见布局

## 文档流

> 文档流（HMTL标签的默认排列方式）：元素是遵循**从下至上**显示，**上面**的元素可以**遮挡下面**的元素，这也就是所谓的元素层级。
>
> **行、块是HMTL标签本身自带的性质**
>
> **块元素**
>
> ​          \- 块元素会在页面中独占一行(自上向下垂直排列)
>
> ​          \- 默认宽度是父元素的全部（会把父元素撑满）
>
> ​          \- 默认高度是被内容撑开（子元素）
>
> **行内元素**
>
> ​          \- 行内元素不会独占页面的一行，只占自身的大小
>
> ​          \- 行内元素在页面中左向右水平排列，如果一行之中不能容纳下所有的行内元素
>
> ​            则元素会换到第二行继续自左向右排列（书写习惯一致）
>
> ​          \- 行内元素的默认宽度和高度都是被内容撑开
>
> **块元素=>行内元素：**
>
> ​	`display:inline` 此时的**width**和**height**属性失效
>
> **行内元素=>块元素：**
>
> ​	`display:block`
>
> ​	 `display:inline-block` 不会独占一行的块元素

### 脱离文档流（float、absolute、fixed）

> 脱离文档流的特点：
>     **块元素**：
>         1、块元素不在独占页面的一行
>         2、脱离文档流以后，块元素的宽度和高度默认都被内容撑开
>     **行内元素**：
>         行内元素脱离文档流以后会变成块元素，特点和块元素一样
>     脱离文档流以后，不需要再区分块和行内了

## **盒模型**

> 使用点：布局方式、绘制等腰三角形，实现水平居中
>
> 记忆技巧：margin、padding内容像，唯独boder花样多

![盒模型](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200724211910785.png)

### 参数个数问题(margin、padding)

4个参数：上 	右 	下 	左

3个参数：上 	左右 	下

2个参数：上下 	左右

一个参数：上下左右

### **margin**

外边距：不可见，但是会影响标签的位置

| 属性            | 值                                      |
| --------------- | --------------------------------------- |
| `margin-top`    | `digit`px  可以是负值，向着相反方向移动 |
| `margin-bottom` |                                         |
| `margin-left`   |                                         |
| `margin-right`  |                                         |

#### 外边距折叠问题

兄弟元素的外边距



> 兄弟元素的外边距折叠规则：（这是有利于开发的，不是我们需要解决的问题）
>
> ​	①兄弟元素间的相邻垂直外边距会取两者之间的较大值（两者都是正值）
>
> ​	②如果相邻的外边距一正一负，则取两者的和
>
> ​	③如果相邻的外边距都是负值，则取两者中绝对值较大的
>
> 父子元素的外边距折叠问题：（**需要解决**，不然会干扰我们设定位置的预期）
>
> ​	父子元素间相邻外边距，子元素的会传递给父元素（上外边距）

解决方式：有设定`float`和`position=absolute`的元素不会产生外边距重叠行为。

### **border**

边框：可见



| 属性                  | 常用值                                                       |
| --------------------- | ------------------------------------------------------------ |
| `border`              | 三个属性全写上去width color style                            |
| `border-width`        | `digit`px                                                    |
| `border-color`        | `colorName` `transparent`(透明) ……                           |
| `border-style`        | 默认值`none`   `solid`实线` dotted `点状虚线`dashed` 虚线`double`双线 |
| 单一方向设置          |                                                              |
| `border-top-width`    |                                                              |
| `border-bottom-width` |                                                              |
| `border-left-width`   |                                                              |
| `border-right-width`  |                                                              |

#### 等腰三角形绘制

```css
        .box1{
            width: 0px;
            height: 0px;
            border: 10px red solid;
            border-top: none;
            border-color: transparent transparent blue transparent;
            /*有颜色的是等腰三角形底边的方向*/
        }
```

### **padding**

| 属性              |           |
| ----------------- | --------- |
| `paddding-top`    | `digit`px |
| `paddding-bottom` |           |
| `paddding-left`   |           |
| `paddding-right`  |           |

### **content**

`width` , `height`

### 水平方向布局

`margin-left + border-left +  padding-left + width +  padding-right + border-right  + margin-right=  父元素的width`

为了保持等式成立，在上面的值中都没有设置成`auto`的情况下，`margin-right`的默认设置成`auto`

#### 水平居中实现

```css
 {
      width:xxpx;
     margin:0 auto;
 }
```



### 垂直方向布局

content的内容会将父元素撑开，`overflow`就是为了设定溢出样式

| 属性         | 值                                                           |
| ------------ | ------------------------------------------------------------ |
| `overflow`   |                                                              |
|              | `visible`:默认值 子元素会从父元素中溢出，在父元素外部的位置显示 |
|              | `hidden`:溢出内容将会被裁剪不会显示                          |
|              | `scroll`:生成两个滚动条，通过滚动条来查看完整的内容          |
|              | `auto`:根据需要生成滚动条                                    |
| `overflow-x` | 设置水平溢出                                                 |
| `overflow-y` | 设置垂直方向溢出                                             |

### 行内元素的盒模型

行内元素是无法设置`width`和`height`

​	行内元素可以设置padding，**垂直方向**padding不会影响页面的布局

​	行内元素可以设置border，**垂直方向**的border不会影响页面的布局

​	行内元素可以设置margin，**垂直方向**的margin不会影响布局

为了解决这个问题，使用`display`属性将行内元素转化为块元素

| 属性         | 值                                                           |
| ------------ | ------------------------------------------------------------ |
| `display`    |                                                              |
|              | `inline` 将元素设置为行内元素                                |
|              | `block` 将元素设置为块元素                                   |
|              | `inline-block` 将元素设置为行内块元素行内块，既可以设置宽度和高度又不会独占一行 |
|              | `table` 将元素设置为一个表格                                 |
|              | `none` 元素不在页面中显示                                    |
| `visibility` |                                                              |
|              | `visible `默认值，元素在页面中正常显示                       |
|              | `hidden `元素在页面中隐藏 不显示，但是依然占据页面的位置     |

### 类似盒模型效果的属性

| 属性                            | 值                                                           |
| ------------------------------- | ------------------------------------------------------------ |
| `box-shadow`包含四个值,设置阴影 |                                                              |
|                                 | 第一个值 水平偏移量 设置阴影的水平位置 正值向右移动 负值向左移动（px） |
|                                 | 第二个值 垂直偏移量 设置阴影的水平位置 正值向下移动 负值向上移动（px） |
|                                 | 第三个值 阴影的模糊半径（px）                                |
|                                 | 第四个值 阴影的颜色（color）                                 |
| `border-radius`设置圆角         |                                                              |
| `border-top-left-radius`        |                                                              |
| `border-top-right-radius`       |                                                              |
| `border-bottom-left-radius`     |                                                              |
| `border-bottom-right-radius`    |                                                              |

**缩写效果**：

 `border-radius`可以分别指定四个角的圆角

​          四个值 左上 右上 右下 左下

​          三个值 左上 右上/左下 右下

​          两个值 左上/右下 右上/左下

​		  一个值全部

### 常见问题

`outline`和`border`的区别：outline内缩，border向外扩张。结论：轮廓不会影响到可见框的大小

盒子尺寸

| 属性         | 值                                                           |
| ------------ | ------------------------------------------------------------ |
| `box-sizing` |                                                              |
|              | 默认值`content-box`                                          |
|              | `border-box`效果：`width=内容区（动态变化）+内边距+边框（水平）` `height=内容区（动态变化）+内边距+边框（垂直）` |

## **position**

### 常用属性：

| 属性              | 值                         |
| ----------------- | -------------------------- |
| `position`        |                            |
|                   | 默认值`static`，未开启定位 |
|                   | `relative`：               |
|                   | `absolute`                 |
|                   | `fixed`                    |
|                   | `sticky`在safari浏览器中是`position: -webkit-sticky;` |
| **offset**偏移量  |                            |
| `left`            |                            |
| `right`           |                            |
| `bottom`          |                            |
| `top`             |                            |
|                   |                            |
| `z-index`元素层级 | 层级越高就显示在“文档流”的越高处 |

### relative

> 偏移量**参照点**是自己在文档流内的左上角的位置

### absolute（脱离文档流）

> 偏移量**参照点包含块**元素的左上角
>
> 效果：行内=>块，块的宽高被内容撑开

包含块概念：最近的祖先元素，当没有祖先元素就是HTML页面（也就是可视窗口）

```html
<div class="1">
        <div class="2"></div>
</div>
```

### fixed（脱离文档流）

> 固定定位：偏移量**参照点**可视窗口的左上角，且不会随滚动条滚动

### sticky

> 粘性定位被认为是相对定位和固定定位的混合
>
> 效果：拉动滚动条时初始不会移动，当内容要**离开可视窗口**时，就会卡在那个位置不在移动
>
> 场景：需要一直存在于屏幕内容，如：导航条、固定题目滚动答案时题目可以是粘性定位

## **float**

> 最初是为了实现文字环绕效果而出现的，也可用作水平布局即实现元素水平排列，但是存在**高度塌陷**的问题
>
> ​	1、浮动元素会完全脱离文档流，不再占据文档流中的位置
> ​	2、设置浮动以后元素会向父元素的左侧或右侧移动，
> ​	3、浮动元素默认不会从父元素中移出
> ​	4、浮动元素向左或向右移动时，不会超过它前边的其他浮动元素
> ​	5、如果浮动元素的上边是一个没有浮动的块元素，则浮动元素无法上移
> ​	6、浮动元素不会超过它上边的浮动的兄弟元素，最多最多就是和它一样高

| 属性    | 值      |
| ------- | ------- |
| `float` |         |
|         | `none`  |
|         | `left`  |
|         | `right` |

### 高度塌陷问题

> 启动浮动布局之后会导致父元素无法被子元素撑开，出现布局混乱的情况。



![高度塌陷](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200725203533740.png)

![高度塌陷处理后](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200725203610593.png)

#### BFC

开启BFC（**block format context**）  [开启BFC的方式](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)

**注**：开启BFC是给高度塌陷**后一个**的元素开启BFC，而不是给他本身。

```html
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .box1{
            border: 10px red solid;
        }
        
        .box2{
            width: 100px;
            height: 100px;
            background-color: #bfa;
            float: left;
        }
        
        /* 处理方式 */
        .box1::after{
            content: '';
            display: block;
            clear: both;
        }
    </style>
</head>
<body>
    <div class="box1">
        <div class="box2"></div>
    </div>
</body>
</html>
```

`clear`属性介绍：清除浮动元素对自己位置的影响

> 原理：设置清除浮动以后，浏览器会自动为元素添加一个上外边距，以使其位置不受其他元素的影响

| 属性    | 值                                      |
| ------- | --------------------------------------- |
| `clear` |                                         |
|         | `left`清除左侧浮动元素对当前元素的影响  |
|         | `right`清除右侧浮动元素对当前元素的影响 |
|         | `both`清除**两侧中最大影响**的那侧      |

#### clearfix

解决外**边距重叠**和**高度塌陷**问题

最终解决方案（代价最小，业界通用，几乎没有代价😛）

```css
.clearfix::before,
.clearfix::after{
    display:block;
    content:"";
    clear:both;
}
```

## **flex**

> **一维布局方式**，通过`display:flex;`开启，即一次只能操作行或者列，而不能同时操作。

**align-items**:垂直方向，侧轴对齐方式 **justify**:两端

### 常用属性

| 属性                                      | 值                                                     |
| ----------------------------------------- | ------------------------------------------------------ |
| `justify-content`水平操作                 |                                                        |
|                                           | `flex-end;`靠右对齐                                    |
|                                           | `center;`居中对齐                                      |
|                                           | `space-evenly;`平分空间                                |
|                                           | `space-between;`两端对齐                               |
| `align-items`垂直轴操作                   |                                                        |
|                                           | `flex-end;`靠下对齐                                    |
|                                           | `center;`居中对齐                                      |
| `flex-direction`                          | ` column;`改变子元素排列方式，从默认的行排列改为列排列 |
| `flex`设置大小的权重，**见下面代码**      |                                                        |
| `flex-basis`指定flex盒子的大小            |                                                        |
| `flex-wrap`指定flex盒子是单行还是多行显示 |                                                        |
|                                           | 默认值`nowrap`单行，存在严重的溢出风险，               |
|                                           | `wrap`多行                                             |
|                                           | `wrap-reverse`多行                                     |

![flex-wrap demo from MDN](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200725210429180.png)



```css
        div.flex1{
            flex: 1;/* 与其他的子元素的比例此处是1/5 */
            width: 50px;
            height: 21px;
            background-color: red;
        }
        div.flex2{
            flex: 3;/* 与其他的子元素的比例此处是3/5 */
            width: 50px;
            height: 21px;
            background-color: rgb(163, 124, 110);
        }
        div.flex3{
            flex: 1;/* 与其他的子元素的比例此处是1/5 */
            width: 50px;
            height: 21px;
            background-color: chocolate;
        }
```

![效果](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200725205154196.png)

## **grid**

> 一种二维布局方式，`display:grid;`开启即可以同时操作行或者列

| 属性                                       | 值                                                           |
| ------------------------------------------ | ------------------------------------------------------------ |
| `grid-template-columns`                    | `xxpx`或者`fr` 是一种权重单位[该属性比较复杂去MDN看比较好](https://developer.mozilla.org/zh-CN/docs/Web/CSS/grid-template-columns) |
| `column-gap`                               | 行间距px                                                     |
| `row-gap`                                  | 列间距px                                                     |
| `grid-template-areas`搭配`grid-area: `使用 | 效果见下方代码和图                                           |
| `align-content`                            | `center`                                                     |
| `justify-content`                          | `center`                                                     |
|                                            |                                                              |
|                                            |                                                              |
|                                            |                                                              |
|                                            |                                                              |
|                                            |                                                              |
|                                            |                                                              |
|                                            |                                                              |
|                                            |                                                              |

```css
        .layout {
            display: grid;
            width: 400px;
            height: 400px;
            grid-template-areas:
                "header header header"
                "sidebar content content"
                "footer footer footer";
        }

        header {
            background-color: aqua;
            grid-area: header;
        }

        aside {
            background-color: aquamarine;
            grid-area: sidebar;
        }

        main {
            background-color: red;
            grid-area: content;
        }

        footer {
            background-color: blue;
            grid-area: footer;
        }
```

![效果](C:/Users/zoulam/AppData/Roaming/Typora/typora-user-images/image-20200725211515950.png)

## 圣杯(Grail)布局(layout)

> 圣杯布局实现效果：





## 双飞翼布局

![圣杯布局](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200724213217610.png)