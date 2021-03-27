# \[css\]入门

## 重要知识速记

```javascript
选择器
    . # div * 
    不用逗号隔开表示同时满足 a.b（选择b）
    a,b 或（两个元素都选择）
    a > b（父子） a b(祖孙含父子)
    a+b(第一个兄弟) a~span（后面的全部兄弟span，不含a）
    [title] [title=abc] 与正则语法类似 [titile^=abc] [title$=abc] [title*=abc]
伪类(:)
    a link visited hover acctive
伪元素(::)

优先级
    !important > inline > id > class/pseudo class > element > * > inherit

盒模型
    content 
    padding
    border 
            style dotted(点状虚线) solid double dashed(虚线)
            color transport
    margin
box-sizing 默认 content-box （width/height 就是content）
                border-box （可见宽高，content+padding+border）

                box-shadow 水平 垂直（px） 半径 颜色
                border-radius
                outline 内缩


子margin干扰父亲的位置，设置float/absolute、兄弟之间会自动折叠取较大的margin

文档流是基于盒模型的概念，一种根据语言排列文字
脱离文档流：float、absolute、fixed

position static 
        relative 自己原本位置
        absolute 父div
        fixed 脱离视口才粘滞
        stickly 固定在屏幕特定位置

        相关属性：z-index

单位
    rem(html min=12px) em px vw vh deg（旋转的属性的单位）
    rgb rgba colorName
    不再缩放出现滚动条：min-width min-height
    calc(100px/2) 50px
    list-style
    cursor
    pointer-events
    opacity 0

display 
    none(删除节点)
    inline-block(可以设置width、height的行元素)
    block
    inline
    flex
    grid

visibility
    visible(默认值)
    hidden(隐藏)
    collapse(折叠)

font:
    size
    line-height (行间距 == line-height - font-size)
    family
    font-weight 字体粗细

font-style
    text-decoration (装饰)
    text-align
    vertical-align
    white-space
    text-overflow

@font-face{
    font-family
    src:url format
}

iconfont

base64文件比正常图片大，但是可以减少http请求

background: -color
            -image : url |
                    linear-gradient(to right , color1 , color2 xxpx, ……)
                    radial-gradient()
            -repeat :repeat | repeat-x | repeaty | no-repeat
            -position：left | top | center | (bottom | right)xx% px
               -attachment（附件）:scroll（随元素滚动） | fixed（固定） | ……
               -size: contain(正常尺寸) | cover（铺满） |......

flex
    容器（父）
    diplay:flex | inline-flex
    ①flex-direction: row(默认) | row-reverse | column | column-reverse
    ②flex-wrap: nowrap | wrap | wrap-reverse
    ③flex-flow: 同时设置上面两个属性
    ④justify-content: flex-start | flex-end | center 
                        | space-between(均匀间隔) 
                        | space-around(左右间距均匀哪怕没有元素)

    ⑤align-item: flex-start | flex-end | center | base-line | stretch (元素高度不等时触发)    
       ⑥align-content: 属性与justify-content完全相同，换行时触发

       元素（子）
       order （默认按html内的元素顺序排列，order可以改变顺序，数字越小越前面【含负数】）
           缩放都是基于剩余空间
       ①flex-grow（成长/增长比例）：
           假设有三个元素，他们的grow分别是 1（1/4） 2（2/4） 1（1/4） 也可以设置px
       ②flex-shrink（缩小/缩小比例）：
           假设有三个元素，他们的shrink分别是 0（不缩放） 1（剩余的1/3） 2（剩余的2/3）
       ③flex-basis（基准/宽度）：默认值auto
           高度
       ④flex：缩写①、②、③
               0 1 auto
       ⑤align-self：父容器设置了全部的对齐方式，但这个属性可以单独设置某个子元素的对齐方式

transition（过渡，css属性少，但是可以使用JavaScript的setTimeout和库的生命周期函数定制动画）
    -property(监听样式)：all | height ……
    -duration(持续时间)： s | ms 
    -timing-function(行为函数)：
                    ease 默认值，慢速开始，先加速，再减速
                    | linear 匀速运动
                    | ease-in 加速运动
                    | ease-out 减速运动
                    | ease-in-out 先加速 后减速
                    | cubic-bezier() 来指定时序函数 https://cubic-bezier.com
                    | steps() 分步执行过渡效果
                        可以设置一个第二个值：
                            end ， 在时间结束时执行过渡(默认值)
                            start ， 在时间开始时执行过渡
    -delay(延迟时间)：s | ms


animation(css属性丰富,但难以使用JavaScript操作)
    -name:自定义
        @keyframes <animation-name>{from(0%){} to(100%){}}
    -duration s | ms
    -delay：s | ms
    -timing-function:同上
    -iteration-count（执行次数）：数字 | infinite（无限次数）
    -direction（方向）：normal（默认值 from => to） 
                    | reverse（to => from）
                    | alternate(from => to 然后反向执行)、
                    | alternate-reverse（to => from 然后反向执行）
    -play-state（动画执行状态）：running（默认值执行）
    -fill-mode（动画填充模式）：none 默认值 动画执行完毕元素回到原来位置
                | forwards 动画执行完毕元素会停止在动画结束的位置
                | backwards 动画延时等待时，元素就会处于开始位置
                | both 结合了forwards 和 backwards


transform（变形，搭配transition实现动画效果）
    -origin（设置缩放原点）：center（默认值）
                        | 20px 20px(下，右)
                        | top left(左上角)
                        | bottom right 60px（右下角，z轴的60px高度）    
    -style: flat(默认值，平面空间)
        | preserve-3d （出现遮挡关系）

    【移动】：e(-50%,-50%) | translateX(xx%) 
    | translateY(xx%) | translateZ(xx%) 百分比是参照自身,也可以填充xxpx


    相关属性：perspective（视距）：xxpx；【也就是z轴高度】

    【旋转】： rotateX() | rotateY() | rotateZ()
        填充 turn(圈数)、deg（度数）、xxpx

    相关属性：backface-visibility（是否显示元素背面）: hidden

    【缩放】： scaleX() 水平方向缩放
             | scaleY() 垂直方向缩放
             | scale() 双方向的缩放
            填充倍数 0.5 原来的1/2，2放大两倍

    【倾斜】：skew() 这个倾斜会让元素变扁
             | skewX()
            | skewY()    

    【矩阵】：matrix
```

[matrix](https://www.zhangxinxu.com/wordpress/2012/06/css3-transform-matrix-%E7%9F%A9%E9%98%B5/)

## 1、去除默认样式

> 在设置css样式前往往需要去除HTML自带的默认样式

### 简单去除

> 没能将无序列表等样式去除

```css
*{
    margin:0;
    padding:0;    
}
```

### 完全去除

[代码获取](https://github.com/necolas/normalize.css)

## 2、选择器

| 选择器 | 选择内容 |
| :--- | :--- |
| `.className` | 类选择器 |
| `#id` | id 选择器 |
| `*` | 通配选择器（选择当前页面所有内容） |
| `div` `span` …… | 直接选取**html**标签名 |
| 复合选择器 |  |
| `div.className`, `div#idname` | 同时满足（and） |
| `A,B` | 单一满足（or） |
| 关系选择器 |  |
| `A>B`或 `A B` | A 是 B 的父亲（选择 B） |
| `A>B>C` 或`A C` | A 是 C 的祖宗（选择 C） |
| `A+span` | span 是 A 的第一个兄弟（选择第一个 span，**不包括 A**） |
| `A~span` | A 后面的全部兄弟（选择 A 的全部 span，**不包括 A**） |
| 属性选择器 |  |
|  | 通常用来操作不存在的元素（使用 dom 生成的内容） |
| `p[title]{}` | 选择 title |
| `p[title=abc]{}` | 选择 titile=abc |
| `p[title^=abc]{}` | 选择 title 以 abc 开头 |
| `p[title$=abc]{}` | 选择 titile 以 abc 结尾 |
| `p[title*=e]{}` | 选择 title 包含 e |
| 伪类 一个冒号 |  |
| `div:first-child` | 选择 div 的第一个子元素 |
| `xx:last-child` | xx 的最后一个子元素 |
| `:nth-child(n)` | 操作所有子元素 |
| `:nth-child(2n)` `:nth-child(even)` | 操作**偶数**位置的子元素 |
| `:nth-child(2n+1)` `:nth-child(odd)` | 操作**奇数**位置的子元素 |
| `:first-of-type` |  |
| `:last-of-type` |  |
| `:nth-of-type()` |  |
| `:not()` `p:not(.fancy) {}` | 选择类名不是 fancy 的 p 元素 |
| a 的伪类（下面的四个属性**存在包含关系**应该从上至下写入不然会失效） |  |
| `a:link` | 正常链接 |
| `a:visited` | 被访问的链接（出于隐私考虑只能设置颜色属性） |
| `a:hover` | 选择鼠标移入时（**其他元素也能用**，较常见的就是按钮） |
| `a:active` | 选择鼠标点击未松开时（**其他元素也能用**，较常见的就是按钮） |
| [更多伪类](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes) |  |
| 伪元素选择器（双冒号） |  |
| 用来选择特定位置的**文本信息**内容，比如实现首字母大写，书内的首个单词或文字实现特定样式 |  |
| `p::first-letter` | p 的表示第一个字母 |
| `p::first-line` | p 的第一行 |
| `p::selection` | p 内的文本 |
| 实现特定内容包裹如`“”`，`《》` |  |
| `::after` | **after**和**before**必须结合`content`属性使用 |
| `div::before{content: '『';}` | 在 div 前面添加『 |

## 3、优先级

`!important > inline > id > class/pseudo class > element > * > inherit`

CSS 优先级森严，同级覆盖关系（根据样式被文档读取的顺序，从上至下），

`!important`往往能不用就不用

## 4、继承关系

## 5、样式的继承

不用特别去记忆，只是在出现**未设置样式**异常时应该想到是样式继承的干扰

> 样式继承是先设置祖宗节点子孙节点也会得到该样式。
>
> 注意：并不是所有的样式都会被继承：

不可继承的：

```text
display、margin、border、padding、background、height、min-height、max- height、width、min-width、max-width、overflow、position、left、right、top、 bottom、z-index、float、clear、table-layout、vertical-align、page-break-after、 page-bread-before和unicode-bidi
```

所有元素可继承：`visibility和cursor`

内联元素可继承：`letter-spacing、word-spacing、white-space、line-height、color、font、 font-family、font-size、font-style、font-variant、font-weight、text- decoration、text-transform、direction`

块状元素可继承：`text-indent和text-align`

列表元素可继承：`list-style、list-style-type、list-style-position、list-style-image`

表格元素可继承：`border-collapse`

## **4、单位**

### size单位

`px` pixel （像素）指的是**CSS像素**，**物理像素**是显示器的最小颗粒单位，两者成比例，比例由浏览器决定

`vh`viewport high

`em` `1em == 当前元素的font-size`

注：HTML内的`font-size`是不能设置为**1~11**之间的值，即使设置了也会被校正成12px，即最小是0px其次是12px

`rem` `1em == 根元素（HTML）的font-size` 默认值是**12px**

`%` 父元素百分比

`fr` gird的特有单位（权重设置）

### color单位

![&#x989C;&#x8272;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201116004937936.png)

```text
name： 
    red、pink、purple、deeppurple、indigo、bule、lgihtbule、
    cyan、teal、green、lightgreen、lime、yellow、amber、orange、
    deeporange、brown、grey、bluegrey
```

```text
rgb
    rgb(255,255,255)//白色 (0,0,0)// 黑色
```

```text
0x 十六进制
    #000 代表#000000 #ff0代表#ffff00
```

```text
rgba
    a透明度(0-1) 0代表完全透明，1完全不透明
    rgba(255,255,255,0.5)
```

```text
hsl
    Hue(色相)saturation(饱和度)lightness(亮度)a透明度(0-1) 0代表完全透明，1完全不透明
    hsl(50%,98,25) //百分号可省略
```

```text
hsla
    hsla(50%,56,25,0.6)
```

## 5、常用属性

Windows下的字体库：`C:\Windows\Fonts`,若是缺少指定字体那么就会无法显示这些字体

`list-style`:设置列表样式

`background-color`

`white-space`:对空白内容的处理方式 [MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/white-space)

`background-position`:对雪碧图进行切图的属性

### 设计图

PSD文件格式是设计图，由多张图片拼接而成，是一种多图层的设计格式。

### font-text-background

| 属性 | 值 |
| :--- | :--- |
| `font-size`指定字体大小 |  |
| `font-family` | 格式：serif 衬线字体sans-serif 非衬线字体monospace 等宽字体 |
|  | 字体库： |
|  | 字体形式**图标字体**、**emoji**…… [更多值](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-family) |
| `@font-face`定义字体族，见下方代码 | **慎用**：存在版权问题 |
| `iconfont` | **单色图标**，通过**下载文件**或者CDN从外部网站引入 |
|  | 常用字体图标 [阿里矢量图表库](https://www.iconfont.cn/help/detail?spm=a313x.7781069.1998910419.15&helptype=draw) [fontawesome](https://fontawesome.com/) |
| `font-weight` 指定字体粗细 | [可选值](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-weight) |
| `font-style` |  |
| 属性简写 | font: 字体大小/行高 字体族 （行高 可以省略不写 如果不写使用默认值） |
| `line-height` | 行间距=行高-字体大小 |
| `text-decoration`装饰 |  |
|  | 默认值`none`  `underline` 下划线`line-through` 删除线`overline` 上划线 |
|  | 可以设置**颜色**  **实线、虚线点、状虚线**等`border-style`也有的的属性 |
|  |  |
| `text-align` 文本的水平对齐方式 | `left`左侧对齐`right`右对齐`center`居中对齐`justify`两端对齐 |
| `vertical-align`设置元素垂直对齐的方式 | `baseline` 默认值 基线对齐`top` 顶部对齐  `bottom`底部对齐`middle` 居中对齐 |
|  | 基线对齐：类似于英语本的下方开始数的第二条线 |
| `white-space`:对空白内容的处理方式 | `normal` 正常`nowrap` 不换行`pre` 保留空白 |
| `text-overflow`对文本溢出内容的处理 | [可选值](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-overflow) |
| 背景 |  |
| `background-image`引入 | `url("xx");` |
| `background-repeat` 背景重复方式 | `repeat-x`**只**沿着x轴方向重复`repeat-y`**只**沿着y轴方向重复`no-repeat` 背景图片不重复 |
| `background-position` | [可选值](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-position) `top` `bottom` `center` `left` `right` |
| `background-origin`背景图片的偏移量计算的原点 | 当使用 `background-attachment`为fixed时，该属性将被忽略不起作用。`border-box` `padding-box` `content-box` |
| `background-clip`设置背景的范围**clip**:修剪，裁剪 | `border-box` `padding-box` `content-box` |
| `background-attachment`背景图片是否跟随元素移动，**attachment**：附件，理解为依附方式 | `scroll` 默认值 背景图片会跟随元素移动`fixed` 背景会固定在页面中，不会随元素移动 |
| `background-size`设置背景图片的大小 | 初始值：`auto` `cover`:填满 `contain` `xxpx` `xx%` [more](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-size) |
| `background:linear-gradient(props)`或`background-image:linear-gradient()`渐变 | 可填入**props**:方向`to left` `to right` `to top` `to bottom` 也可组合使用`to left top` 或角度：`45deg`渐变轴为45度 |
|  | 后面填入颜色参数（不限）颜色后面可以填入百分比 |
|  | 示例：`background:linear-gradient(to right, red 0%, orange 25%, yellow 50%, green 75%, blue 100%);` |
| `background-image:repeating-linear-gradient()`由重复线性渐变组成的image | 参数同上， |
| `radial-gradient()`径向渐变，与普通渐变不同的是，这种渐变是一种放射效果 | [more](https://developer.mozilla.org/zh-CN/docs/Web/CSS/radial-gradient) |
|  | 语法：`radial-gradient(大小 at 位置, 颜色 位置 ,颜色 位置 ,颜色 位置)` |
|  | 示例：`background-image: radial-gradient(farthest-corner at 100px 100px, red , #bfa)` |

#### @font-face

```css
        @font-face {
                /* 指定字体的名字 */
            font-family:'myfont' ;
            /* 服务器中字体的路径 */
            src: url('./font/ZCOOLKuaiLe-Regular.ttf') format("truetype");
        }
        p{
            font-family: myfont;//指定内置字体族
        }
```

#### font的简写属性

```css
box1{
    /*
    粗体
    斜体
    font-size：50px
    line-height：2*50px
    ……
    serif（衬线字体即又勾勒）
    */
    font: bold italic 50px/2  微软雅黑, 'Times New Roman', Times, serif;
    /*即使不写也会由默认值，所以简写属性需要卸载最前面，不然会覆盖前面单独设置是属性*/
}
```

#### fontawesome 使用步骤

图标字体优势：

制作方式：**SVG**

使用矢量图绘制（参照字体的制作方式即icon font）

好处：文件比picture小，灵活性强：可以轻松改变大小和颜色。

**缺陷：只能适用于单色图标**

为了方便记忆，**iconfont**通常使用`<i></i>`包裹

> 1.下载 [https://fontawesome.com/](https://fontawesome.com/)
>
> 2.解压
>
> 3.将css和webfonts移动到项目中
>
> 4.将all.css引入到网页中
>
> 5.使用图标字体
>
> 方式一
>
> * 直接通过类名来使用图标字体\(查阅文档\)
>
>   class="fas fa-bell"
>
>   class="fab fa-accessible-icon"
>
>   &lt;!--
>
> 方式二
>
> 通过实体来使用图标字体：
>
> &\#x图标的编码;
>
> ```markup
> <link rel="stylesheet" href="./fa/css/all.css">
> <span class="fas">&#xf0f3;</span>
> ```

#### 阿里字体使用

> 1、cdn引入
>
> 2、下载css文件

### transition\(过渡\)

> 是一个渐渐变化的过程，而不是瞬间变成，且需要触发条件（即某些属性的变化）。
>
> 可过渡的数值：xxxpx 颜色 角度等可以计算的值都可以过渡
>
> 注：`auto`不可以
>
> 通过对盒模型的`margin`这种改变位置的属性进行设置可以实现动画效果

| 属性 | 可选值 |
| :--- | :--- |
| `transition` | `all 2s` |
|  | 可以同时设置过渡相关的所有属性，只有一个要求，如果要写延迟，则两个时间中第一个是持续时间，第二个是延迟 |
| `transition-property`指定要执行过渡的属性 | `height width ……` 中间用逗号隔开 |
|  |  |
| `transition-duration`指定过渡效果的持续时间 | `2s` `2000ms`可以对上面的属性分别设置时间 |
|  |  |
| `transition-timing-function`过渡的时序函数**（兼容性存在问题）** | `ease` 默认值，慢速开始，先加速，再减速`linear` 匀速运动`ease-in` 加速运动`ease-out` 减速运动`ease-in-out`先加速 后减速 |
|  | `cubic-bezier()`通过贝塞尔曲线指定[参数设置](https://cubic-bezier.com)                                        `steps()`分步执行，是一种**切分的瞬变** 参数`(步数，end/start)`    `(2，start)`:开始的时候立刻执行，而不是第一步时间结束后执行 |
| `transition-delay`过渡效果的延迟，等待一段时间后在执行过渡 | `2s`:先等待2秒再执行过渡 |

### animation（动画）

| animation |  |
| :--- | :--- |
| `@keyframes framesName{}`  定义关键帧的名字：下方代码是设置为test | `from` `to` 等价于`0%`和`100%` |
| `animation-name`:要对当前元素生效的关键帧的名字 |  |
| `animation-duration`动画运动时间 |  |
| `animation-delay`动画开始时延 |  |
| `animation-timing-function`动画执行的速率函数 |  |
| `animation-iteration-count` 动画执行的次数 | `xx`   次数 `infinite` 无限执行 |
| `animation-direction`指定动画运行的方向 | `normal` 默认值 从 from 向 to运行 每次都是这样                                        `reverse` 从 to 向 from 运行 每次都是这样 |
|  | `alternate` 从 from 向 to运行 重复执行动画时反向执行\(**每个周期的执行方向都相反**\)                  `alternate-reverse` 从 to 向 from运行 重复执行动画时反向执行 |
| `animation-play-state`设置动画的执行状态 | `running` 默认值 动画执行 `paused` 动画暂停 |
| `animation-fill-mode`动画的填充模式**（即动画执行完之后元素停留的方式）** | `none` 默认值 动画执行完毕元素回到原来位置 |
|  | `forwards`动画执行完毕元素会停止在动画结束的位置 |
|  | `backwards` 动画延时等待时，元素就会处于开始位置`from`,效果是同名的属性会被`from`设置的属性覆盖 |
|  | `both` 结合了forwards 和 backwards |
| [more](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation) |  |

#### @keyframes

```css
        /* 
        动画
            动画和过渡类似，都是可以实现一些动态的效果，
                不同的是过渡需要在某个属性发生变化时才会触发
                动画可以自动触发动态效果

            设置动画效果，必须先要设置一个关键帧，关键帧设置了动画执行每一个步骤
        */
        @keyframes test {
            /* from表示动画的开始位置 也可以使用 0% */
            from{
                margin-left: 0;
                background-color: orange;
            } 

            /* to动画的结束位置 也可以使用100%*/
            to{
                background-color: red;
                margin-left: 700px;
            }
        }
```

### transform （变形）

#### `transform-style`

`transform-style:preserve-3d;`将变形效果设置为3d

实现一些**拟态效果**

> 变形就是指通过CSS来改变元素的形状或位置
>
> * 变形不会影响到页面的布局（只会覆盖，但是不会挤走其他元素）
> * transform 用来设置元素的变形效果
> * 平移：
>
>   `translateX()` 沿着x轴方向平移（水平方向）**负值左移，正值右移**
>
>   `translateY()` 沿着y轴方向平移（垂直方向）**负值上移，正值下移**
>
>   `translateZ()` 沿着z轴方向平移（屏幕与用户的轴）**正值，视觉效果变大，离用户更近**
>
>   Z轴需要设置**视距**`perspective: xxxpx;`才能生效
>
> * 平移元素，百分比是相对于自身计算的

#### 实现绝对居中

```css
        .box3{
            background-color: orange;
            position: absolute;
            left: 50%;
            top: 50%;
            transform: translateX(-50%) translateY(-50%);
        }
```

#### 实现上浮动画

```css
    body{
            background-color: rgb(236, 236, 236);
        }

    .box4, .box5{
            width: 220px;
            height: 300px;
            background-color: #fff;
            float: left;
            margin: 0 10px;
            transition:all .3s;
        }

        .box4:hover,.box5:hover{
            transform: translateY(-10px) translateX(10px);
            box-shadow: 0 0 10px rgba(0, 0, 0, .3)
        }
```

#### 旋转

> **图片的坐标轴会根据坐标的旋转而改变**
>
> `transform: rotateZ(.25turn);` 沿着Z轴旋转0.25圈
>
> `transform: rotateY(180deg) translateZ(400px);`沿着Y轴旋转180度，再Z轴平移400px（变大）
>
> `transform: translateZ(400px) rotateY(180deg);`与第二行颠倒顺序之后**视觉效果变小**

#### `backface-visibility: hidden;`

设置图片背面隐藏，在旋转过程中可以让图片变成单面图片。

#### 缩放

> `transform:scale(1.2)` 填入参数缩放倍数，此处是缩放为原来的1.2倍
>
> `scaleX()` 水平方向缩放
>
> `scaleY()` 垂直方向缩放
>
> `scaleZ()` 视距方向缩放，需要配合3d效果`transform-style:preserve-3d;`才能看到效果
>
> `scale()` 双方向的缩放

#### transform-origin

变形原点设置，默认值是`center`

`transform-origin:20px 20px;`

### cursor（光标）

> 鼠标悬停在某处时的光标形态
>
> [more](https://developer.mozilla.org/zh-CN/docs/Web/CSS/cursor)

### vertical-align\(垂直排列\)

> 用来指定行内元素（inline）或表格单元格（table-cell）元素的垂直对齐方式。
>
> [more](https://developer.mozilla.org/zh-CN/docs/Web/CSS/vertical-align)

### opacity（透明）

> 对`img`、`background`进行设置，0~1 0完全透明

### min-width、min-height

> 最小宽度和最小高度，当缩小到这个值时网页不在缩放，而是出现滚动条不完全显示

## 6、一些推荐

### 文章推荐

[我写CSS的常用套路](https://juejin.im/post/5e070cd9f265da33f8653f00)：这个被我设置为css.json文件的设置

[CSS技巧](https://juejin.im/post/5aab4f985188255582521c57)

[【 FlutterUnit 食用指南】 开源篇](https://juejin.im/post/5e94e4d3f265da480836b943#heading-11)

[less入门](https://juejin.im/post/5a2bc28f6fb9a044fe464b19#heading-13)

### 网站推荐：**好像需要翻墙**

[素材站](https://material.io/)

[取色网站](https://www.materialpalette.com/colors)

### 工具推荐

截图去色工具 `snipaste`

![snipaset](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201116004416419.png)

