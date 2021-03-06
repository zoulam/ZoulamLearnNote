# 前端入门

## 浏览器

[浏览器的工作原理：新式网络浏览器幕后揭秘](https://www.html5rocks.com/zh/tutorials/internals/howbrowserswork/)

[回流（reflow）和重绘（Painting）](https://zhuanlan.zhihu.com/p/52076790)

[触发重绘的属性列表](https://gist.github.com/paulirish/5d52fb081b3570c81e3a)

[浏览器工作原理：从输入URL到页面加载完成](https://github.com/amandakelake/blog/issues/55)

[从输入url到页面加载完成的过程](https://zhuanlan.zhihu.com/p/34288735)

[从浏览器多进程到JS单线程，JS运行机制最全面的一次梳理](https://segmentfault.com/a/1190000012925872)

>  这是一个开放性的问题：（建议回答时简要概括，再详细展开自己熟悉的部分）
>
> ​	1、输入URL
>
> ​	2、根据缓存机制读取缓存数据
>
> ​	3、DNS解析
>
> ​			DNS从右往左解析URL访问不同级别的服务器，解析出具体的ip地址
>
> ​			基本流程：浏览器缓存 -> 系统缓存 -> 路由器缓存 -> ISP(运营商)DNS缓存 -> 根域名服务器 -> 顶级域名服务器com -> 主域名服务器的顺序
>
> ​			浏览器缓存可通过在浏览器输入`chrome://net-internals/#dns`查看
> ​			系统缓存在`/etc/hosts`文件中（linux系统）
>
> ​			这里的**浏览器缓存**也可以展开。
>
> ​	4、TCP连接
>
> ​			三次握手
>
> ​	5、发送http请求
>
> ​	6、接受响应，判断状态码选择处理方式
>
> ​	7、判断缓存
>
> ​	8、解码（根据文件类型（`Content-Type`）解析）、渲染
>
> ​	9、连接结束
>
> ​				四次挥手
>
> other：浏览器线程：
>
> ​		JS线程、渲染进程、

浏览器缓存机制：

<img src="https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/v2-b25c1588fb03898fccd8d4a1cc4931c1_1440w.jpg" alt="cache" style="zoom: 33%;" />

渲染过程

<img src="https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/webkitflow.png" alt="webkit" style="zoom: 67%;" />

合成

<img src="https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/render-tree-construction.png" alt="&#x5C06; DOM &#x4E0E; CSSOM &#x5408;&#x5E76;&#x4EE5;&#x5F62;&#x6210;&#x6E32;&#x67D3;&#x6811;" style="zoom: 67%;" />

1. 解析HTML，生成DOM树，解析CSS，生成CSSOM树（取决于你在html文档的顺序）
2. 将DOM树和CSSOM树结合，生成渲染树\(Render Tree\)
3. `Layout/reflow`(回流\):根据生成的渲染树，进行回流\(Layout\)，得到节点的几何信息（位置，大小）
4. `Painting`\(重绘\):根据渲染树以及回流得到的几何信息，得到节点的绝对像素
5. `Display`（显示）:将像素发送给GPU，展示在页面上。（这一步其实还有很多内容，比如会在GPU将多个合成层合并为同一个层，并展示在页面中。而css3硬件加速的原理则是新建合成层，这里我们不展开，之后有机会会写一篇博客

【`visibility`和`opacity`隐藏的是可见节点，**display:none是不可见节点，不在dom结构中**】

**reflow**：可见节点和其样式结合的再**与设备的viewport**计算出准确单位`px`的过程

**painting**：节点的位置信息发生改变，新增，删除，修改【尺寸】，等等

消耗性能的原因：浏览器【**现代浏览器是达到临界值，类似于内存爆掉的情况**】会在绘制过程中**清空队列重新绘制**

**回流一定会触发重绘，而重绘不一定会回流**

### 优化

1、合并js的样式修改，先创建样式文本再**追加**到 `style.cssText+=` 上

2、批量修改DOM，①脱离文档流②进行复杂修改③回到文档流

2.1、使用文档碎片（createDocumentFragment）的方式创建节点，全部添加到Fragment完成后再添加到页面

2.2、`display:none` =&gt;更新节点内容=&gt;`display:block`

3、缓存值相同值

>  对相同值的操作进行缓存，全部操作完成才触发重绘。

```javascript
// 未缓存
function initP() {
    for (let i = 0; i < paragraphs.length; i++) {
        // box.offsetWidth 上次循环的修改完成【重绘】才能开始这次循环
        paragraphs[i].style.width = box.offsetWidth + 'px';
    }
}
// 缓存
const width = box.offsetWidth;
function initP() {
    for (let i = 0; i < paragraphs.length; i++) {
        paragraphs[i].style.width = width + 'px';
    }
}
```

4、css3硬件加速【GPU加速】

## HTML

### [H5新增内容](https://developer.mozilla.org/zh-CN/docs/Web/Guide/HTML/HTML5)

### 基本标签学习

```javascript
<scirpt defer="defer" /> 比下面下个脚本更先执行
<scirpt defer="defer" />
```

`defer` 延迟，**HTML5规范要求**，两个defer保证顺序（HTML文档内的顺序）且 保证在`DOMContentloaded`之前，**但事实并非如此**。

`async` 异步【下载不会阻塞，用于存放不修改DOM的代码】，两个异步无先后顺序， `onload`之前，`DOMContentloaded`**之前或之后**

```javascript
!DOCTYPE html // 标准模式 不写就是兼容模式（模拟老浏览器的能力）
html lang
head
    meta
        charset
        name:keywords、description、viewport    content
        // 完美视口
<meta name="viewport" content="width=device【设备/终端】-width, initial-scale【比例】=1.0">
        http-equiv content
    title
link （href）
script defer="defer" async type="text/script" "module" src XHTML中 async="async"
style rel="stylesheet" href
body

// @import 是阻塞的
// 当浏览器遇到href会并行下载资源并且不会停止对当前文档的处理。(同时也是为什么建议使用 link 方式加载 CSS，而不是使用 @import 方式)
// 当浏览器解析到src ，会暂停其他资源的下载和处理，直到将该资源加载或执行完毕。(这也是script标签为什么放在底部而不是头部的原因)

src :script source img input(image) iframe // src source，用于替换当前内容，
href:link a // Hypertext Reference，用于在当前文档和引用资源之间确立联系

// 行元素
// 不占满一行，内容填充，不可设宽高
a b span 
img【可替换元素，虽然是行元素但是能设置宽高】
strong sub sup button input label select textarea
// 块元素
// 可设宽高，独占一行
div ul【无序列表】 ol【有序列表】 li【列表元素】
dl【定义列表】 dl的列表元素：（dt dd） h1 h2 h3 h4 h5 h6（最多到6级标题） p

// h5语义化标签
hgroup header footer nav aside section【段落】 article output label
strong del select progress【精度】
source > audio source > video
// 自闭标签，或者说是空元素，h5可以省略 / <img>
常见：br hr img input link meta 冷门：area ifame source


from(块元素) 属性：action entype method
input(行、自闭)  name value autocomplete=“off” readonly autofocus
                type:file text color checkbox(多选、依靠name分组) radio（单选）
                    button submit reset date random search email
          （行元素）<button></button> 非自闭
（块元素）
// 嵌套关系，在vue中使用 :is动态组件来保证嵌套
table: thead tbody tfoot
                 tr(行：table row)
                     td（列）(colspan="2"向下合并2行、rowspan="2"向右合并2列)
/* 指定边框之间的距离 */
border-spacing: 0px;
/* 设置边框的合并 */
border-collapse: collapse;
```

### 冷门标签

>  折叠语法，`code`片段

```JavaScript
<details>
    <summary>折叠标题</summary>
	…… 此处填入展开后显示的内容，可以不带标签
</details>

<details>
    <summary>折叠标题</summary>
    <p>加法函数</p>
    <pre><code>
        function add (a, b){
            return a + b
        }
    </code></pre>
</details>
```

> ​	 `map`，`area` [区域映射](https://www.cnblogs.com/okgoodman/p/8586524.html)
>
> ​	`coords` 坐标
>
> ​	`shape`形状 `circle` `rect` `poly`多边形
>
> ​	`href`
>
> ​	`target`打开标签的行为，与`a`标签的一致
>
> ​			`_blank`空白页
>
> ​			`_self`当前页（**默认**）
>
> ​			`_parent`:在父框架集中打开被链接的文档
>
> ​      	`_top`：在整个窗口中打开被链接文档
>
> 　　　`framename`:在指定框架中打开被链接文档
>
> 比起a标签的**优势**： `area`可以嵌套`area` ，`a`标签则不可以

```javascript
    <img src="https://pic1.zhimg.com/v2-40f2e8d4e82ed0f6e6b050dd47fb8d12_b.jpg" width="444" height="395" alt="中国地图"
        usemap="#province" />
    <map name="province" id="province">
        <area shape="rect" coords="80,112,110,125" alt="新疆" target="_blank"
            href="https://baike.baidu.com/item/%E6%96%B0%E7%96%86/132263?fr=aladdin">
        <area shape="rect" coords="77,209,110,229" alt="西藏" target="_blank"
            href="https://baike.baidu.com/item/%E8%A5%BF%E8%97%8F/130045">
        <area shape="rect" coords="150,176,185,192" alt="青海" target="_blank"
            href="https://baike.baidu.com/item/%E9%9D%92%E6%B5%B7/31638">
        <area shape="rect" coords="197,236,235,261" alt="四川" target="_blank"
            href="https://baike.baidu.com/item/%E5%9B%9B%E5%B7%9D/212569">
        <area shape="rect" coords="170,300,211,325" alt="云南" target="_blank"
            href="https://baike.baidu.com/item/%E4%BA%91%E5%8D%97/206207">
        <area shape="circle" coords="227,200,8" alt="甘肃"  target="_blank"
        href="https://baike.baidu.com/item/%E7%94%98%E8%82%83">
        <area shape="circle" coords="240,177,5" alt="宁夏" target="_blank"
         href="https://baike.baidu.com/item/%E7%94%98%E8%82%83">
        <area shape="circle" coords="285,133,8" alt="内蒙古" target="_blank"
         href="https://baike.baidu.com/item/%E7%94%98%E8%82%83">
    </map>
```

## CSS

[30分钟学会flex](https://zhuanlan.zhihu.com/p/25303493)

[那些你总是记不住但又总是要用的css](https://zhuanlan.zhihu.com/p/231014167)

[你需掌握的CSS知识都在这了](https://zhuanlan.zhihu.com/p/231014167)

[相对单位在线转换网站](http://pxtoem.com/)

[从矩阵与空间操作的关系理解CSS3的transform（科普文）](https://zhuanlan.zhihu.com/p/50525974)

[字节跳动最爱考的前端面试题：CSS 基础](https://juejin.cn/post/6936913689115099143)

>  `transition`  中文释义：**过渡**，`JavaScript`写动画的属性，用于监听指定的样式变化做出行为。
>
> `transform`变形，放大缩小，旋转等效果的来源。
>
> `animation`动画，属性丰富，直接用`css`操控简单，无法使用JS设置。

```JavaScript
各种css
    style内的css，先用选择器获取，再写
    .test{
        background-color: red;
    }
    JavaScript中的css
    node.style.backgroundColor = 'red'
    html中的行内css
       <div style="background-color: red;width: 100px;height: 100px;"></div>
    vue中的行内css，外面双引号，里面单引号
    <h1 :style="{color:'red','font-weight':200}">这是一个H1</h1>
    react中的行内css(对象形式)
    <MyComponent className="test" style={{backgroundColor:"red"}} />
```

```javascript
link 是 html标签，除了css还能引入图标，顺序加载，可以使用JS的DOM操作
@import是css模块化语法，页面加载完毕后加载

选择器
    . # div * // 类名 id 特定标签 全部标签
    不用逗号隔开表示同时满足 a.b（选择b）
    a,b 或（两个元素都选择）
    a > b（父子） a b(祖孙含父子)
    a+b(第一个兄弟) a~span（后面的全部兄弟span，不含a）
    [title] [title=abc] 与正则语法类似
    正则选择器（性能较差慎用）
    [titile^=abc] [title$=abc] [title*=abc]
伪类(:) pseudo-class
    //  就像添加了特定的类名
    a link visited hover active 【必须按顺序设置存在覆盖关系】
    :first-child :last-child
    :nth:child(n) :nth:child(2n)偶数 :nth:child(2n+1)奇数 :nth:child(3n)3的倍数
    :p:not(.pointer) {} 选择类名不是pointer的
伪元素(::) pseudo-element
    // 就像是添加了特定元素节点
    selection 被选中文本 first-line first-letter首字母
    after{content: '』';} before{content: '『';}

优先级
    !important > inline > id > class || pseudo class > element（即直接选择标签） > * > inherit
	可以记忆为：越不相干等级越低，破坏性越大等级越低
	!important > inline > id(都是属于唯一性的选择)
	class/pseudo class（范围大不唯一）
    element（即直接选择标签） > * （十分模糊，甚至不知道选择哪些标签）
	inherit（几乎无法控制，优先级低易覆盖）

盒模型
    属性缩写
        上 右 下 左
        上下 左右
        上 左右 下
        上下左右
    content
    padding
    border
        style dotted(点状虚线) solid double dashed(虚线)
        color transport ……
    margin
box-sizing 默认  content-box （width、height 就是content，默认值）
                border-box （可见宽高，content+padding+border）【IE浏览器盒模型的默认值】

                box-shadow 水平 垂直（px） 半径 颜色
                border-radius 圆角
                outline【轮廓】上面的属性与border一致，不过效果是内缩类似于设置 border-box

可继承的属性【不想记，当样式不同于预期时需要考虑】

子margin干扰父亲的位置，设置float/absolute、兄弟之间会自动折叠取较大的margin

文档流是基于盒模型的概念，一种根据语言排列文字
脱离文档流：float、absolute、fixed

position static
        relative 自己原本位置
        absolute 父div
        fixed 将脱离视口才粘滞
        stickly 固定在屏幕特定位置 可以理解为relative和fixed的组合

        相关属性：z-index

单位
    rem(html min=12px) em【父元素的font-size】 px
    以iphone6的375px *667px为例
    vw【100vw=375px】 vh【100vw=667px】 deg（旋转的属性的单位）
    rgb rgba colorName
    hsl hsla
    不再缩放出现滚动条：min-width min-height
    calc(100px/2) 50px

常用属性
    list-style
    cursor
    pointer-events
    opacity:0 【透明】

display
    none(隐藏节点)
    inline-block(可以设置width、height的行元素)
    block(行元素边块元素)
    inline
    flex
    grid

visibility
    visible(默认值)
    hidden(隐藏)
    collapse(折叠)

font:
    -size
    line-height (行间距 == line-height - font-size)
    family
    font-weight 字体粗细

font-style
    text-decoration (装饰) 样式 颜色 风格 粗细

        text-decoration-line
        文本修饰的位置, 如下划线underline，删除线line-through
        text-decoration-color
        文本修饰的颜色
        text-decoration-style
        文本修饰的样式, 如波浪线wavy实线solid虚线dashed
        text-decoration-thickness
        文本修饰线的粗细

    text-align【行元素水平方向排列，相对父元素的排列方式】
        justify文字向两侧对齐
    vertical-align【行元素，相对父元素垂直方向的排列方式】
        center
    white-space

    text-overflow

@font-face{
    font-family
    src:url format
}

iconfont

base64文件比正常图片大，但是可以减少http请求（等于是分开的图片请求合并到html内）

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
    ①flex-direction（方向）: 【设置主轴方向】
                    row(默认) 左到右
                | row-reverse 右到左
                | column 上到下
                | column-reverse 下到上
    ②flex-wrap【换行行为】:
                    nowrap（默认） 不换行
                | wrap 向下换行
                | wrap-reverse 向上换行
    ③flex-flow: 同时设置上面两个属性
    ④justify-content【主轴排列】:
                    flex-start 左对齐（起点对齐）
                | flex-end 右对齐（终点对齐）
                | center 居中对齐
                | space-between(均匀间隔)
                | space-around(左右间距均匀哪怕是边缘元素左右)

    ⑤align-item【辅轴排列】:
                    flex-start 起点对齐
                | flex-end 终点对齐
                | center 居中对齐
                | base-line 基线对齐
                | stretch【拉伸】 (元素高度不等时触发)
    ⑥align-content: 属性与justify-content完全相同，换行时触发

       元素（子）
       order 【顺序】（默认按html内的元素顺序排列，order可以改变顺序，数字越小越前面【含负数】）
           缩放都是基于剩余空间
       ①flex-grow【成长/增长比例】：
           假设有三个元素，他们的grow分别是 1（1/4） 2（2/4） 1（1/4） 也可以设置px
       ②flex-shrink（缩小/缩小比例）：
           假设有三个元素，他们的shrink分别是 0（不缩放） 1（剩余的1/3） 2（剩余的2/3）
       ③flex-basis（基准/宽度）：默认值auto，即盒子的宽高为【content-box】 width或height
           高度
       ④flex：缩写①、②、③
               0【有空位也不方法】 1【缩小比例为1】 auto 用自身的width或height【视主轴而定】
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
    -duration【持续事件】 s | ms
    -delay【延迟】：s | ms
    -timing【定时调速】-function:同上
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

## JS

### 1、types

![died area](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201124172104259.png)

```javascript
声明方式
var(变量提升、全局作用域)
let(块级作用域,暂时性死区)
const（必须初始化，暂时性死区，基本数据类型不可修改，引用数据类型的元素可修改）

基本数据类型（必须小写、不然会与包装类重名）
number（包含以下内容）：
(float int Infinity NaN 【-2 ** 53 ~ 2 ** 53 -1】【1e6 == 1000000】 1e9 + 7)
string undefined null bigInt symbol boolean
bigInt声明方式 BigInt(15) 或 15n
Symbol使用方式 Symbol(1) 
		// 当需要大量使用Symbol时，则可以直接存储到变量上
		const symbol = Symbol.for
        // for注册的变量会到全局去，注册新值会检查旧的再考虑是否创建，存在则不创建
        let a = symbol('a')
        let b = symbol('a')
        console.log(a == b) // true
		console.log(Symbol(15) == Symbol(15));// false
		{
           [Symbol(1)]:function(){}
        }
包装类：Number Boolean String，存在包装类，这三个也能使用.function的语法

真值：
    number （!0 && !NaN）
    object 非null
    Boolean true
    string 非空字符串
    Symbol
    bigint

假值
    0 NaN
    ""
    undefined
    null
// 任何对象比较都是不一样的，比较的是地址值,当使用赋值的方式进行浅拷贝的时候就返回true
// 下面是真值，表示是否会继续运行
if([] == []) // false
if({} == {}) // false
if([] == false) //true
    
let arr = [1, 2, 3, 4]
let arr1 = arr
console.log(arr1 === arr) // true
// 对引用值的赋值拷贝都是浅拷贝，后续修改会修改到原来的值
let obj1 = { name: 'zoulam' }
let obj2 = obj1
obj2.name = 'lala'
console.log(obj1, obj2)//{ name: 'lala' } { name: 'lala' }



判断方式
typeof ---- 小写 function(){}:(function) null:(object) 基本数据类型
Object.prototype.toString.call()---- [object 大写] 精准
instanceof ---- 返回boolean，任何引用值instanceof Object都是true
constructor ---- 获取构造器 [Function xx]

常用代码段，判断是否为引用数据类型
typeof xx == "object" && xx !== null

隐式类型转换（除了字符串拼接都是转化为数字）
+ 结果一定是 string类型
(非加号运算符) - * / %        string => number(NaN)
(正号/负号)+a -a             string => number(NaN)
(自减、自加/减等于、加等于) a++ a-- --a ++a a += 1 a -=1 string => number(NaN)
(比较运算符> < == !==) 字符串转化为ASCII码 从左到右（带数量级的比较）string => number

 
// JS的底层默认是64位的浮点数存储数据，运算时再转化位32位，所以位运算不一定高效
(位运算 & | ~ >> << ^ >>>) string => number
& 全1为1          全是1  就是1
| 一个1就是1        有1   就是1
^ 按位异或 跟|相反  0或1  就是1 【严格或】
~ 按位非
    ~a == -(a+1) ~15 == -16
<< * 2
>> / 2 向下 3 >> 1 == 1
>>> 正数 * 2 负数会变成超大正数(32位数字)
let num = (1, 2, 3) // 逗号运算符会取出最后以为数，即num = 3


(【短路】逻辑运算符 || && !) 返回值是中断值（从左到右）
// 或的第一个达到true就返回
// 且的第一个达到false或者全为true（返回最后一个）
let bool = false
let bool1 = 2
const a = (bool || 1)
console.log(a) // 1
const b = (bool1 || 1)
console.log(b) // 2
let ans = (!22) // !运算后都是boolean值
console.log(ans) // false

 
Number(num) 不断尾巴,直接NaN                Number('97g') NaN
parseInt(num, radix) 断尾                    parseInt('97g') 97
parseFloat(num) .toFixed() .toPrecision()

// undefined 表明声明但是未初始化（不是合法的JSON数据）
// null 表示空引用（但是JavaScript中是特殊的空对象，是合法的JSON数据）
undefined == null true 其他为false
```

#### **var导致的问题**

```javascript
function foo() {
    for (var i = 0; i < 5; i++) {
        setTimeout(() => {
            console.log(i)
        }, i * 1000)
    }
}

foo()
```

```
输出
每隔一秒输出一个5
出现原因：var 没有块级作用域，所以循环中对i的修改都是对同一个i修改，sto是在for循环之后进行的，所以i已经全部变成5了
```

```javascript
期望每隔一秒输出 0 1 2 3 4
1、var 修改为let
2、将每一次循环的i保存起来
3、立即执行函数 immediately-invoked function expression 【其实是闭包原理】
function foo() {
    for (var i = 0; i < 5; i++) {
        (function (i) {
            setTimeout(() => {
                console.log(i)
            }, i * 1000)
        })(i)
    }
}
foo()
```



### 2、proto

`prototype`是函数（构造函数）特有结构

`__proto__`是对象属性是原型链的链条，即每个对象都能取到 `Object.prototype`上的`toString`方法打印类型。

原型链的终点是 `Object.prototype`

[说说原型（prototype）、原型链和原型继承](https://zhuanlan.zhihu.com/p/35790971)

<img src="https://pic1.zhimg.com/v2-2e8ec703287854d174483ba5f9f937cf_1440w.jpg?source=172ae18b" alt="prototype" style="zoom: 67%;" />

```javascript
function myNew(Func, ...args) {
    let instance = {}
    Object.setPrototypeOf(instance, Func.prototype)
    // instance.__proto__ = Func.prototype // 上面那行代码与这行等价
    let res = Func.apply(instance, args)// 这里需要改变构造函数的this指向
    if (typeof res === 'function' || typeof res === 'object' && res !== null) {
        return res
    }
    return instance
}

function myInstanceof(obj, Func) {
    let proto = obj.__proto__
    let prototype = Func.prototype
    while (proto == null) {
        if (proto == prototype) {
            return false
        }
        proto = proto.__proto__
    }
    return false
}

Object.myCreate = function (p) {
    function f() { };
    f.prototype = p;
    return new f();
}
```

### 3、闭包

> ​	函数特有的性值，将内部的变量（可以是函数、变量等）拷贝出去，给新的变量，每次创建新的变量都是独一无二的，vue组件中的， `data () {return {value}}`，就是利用闭包实现每个子组件的数据独立的。
>
> ​	用于函数内部值方法和属性复用。

```JavaScript
function calculator(val) {
    let obj = {
        a: 1,
        b: 2,
        c: {
            name: 'zoulam'
        }
    }
    let init = val
    function plus(b) {
        init += b
        return init
    }
    function multi(b) {
        init *= b
        return init
    }
    return {
        add,
        multi,
        obj
    }
}

let ca1 = calculator(1)
console.log(ca1.plus(2)) // 3
console.log(ca1.plus(2)) // 5
console.log(ca1.plus(2)) // 7
let ca2 = calculator(1)
console.log(ca2.multi(2)) // 2
console.log(ca2.multi(2)) // 4
console.log(ca2.multi(2)) // 8
ca1.obj.a = 2
console.log(ca1.obj.a) // 2
console.log(ca2.obj.a) // 1
ca1.obj.c.name = 'zhangsan'
console.log(ca1.obj.c.name) // zhangsan
console.log(ca2.obj.c.name) // zoulam
```

### 3、异步编程

[9k字 | Promise/async/Generator实现原理解析](https://juejin.cn/post/6844904096525189128#heading-14)

> ​	在不阻塞同步代码的情况能保证一定顺序执行某些代码块，被称为异步代码，通常是引擎实现的EventLoop做的，注意：EventLoop不再ECMAScript标准范围内。

```javascript
console.log('sync code 1')
let p = new Promise((res, rej) => {
	res('i am async code')
})

async function test() {
	const value = await p // 此处会一直等到value  返回才继续执行
    // 虽然是同步代码，但是在异步代码块中会被阻塞
	console.log(value)
    console.log('sync code 3')
}

test()

setTimeout(() => {
	console.log('sto code')
})
console.log('sync code 2')
```

<img src="https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201127024232718.png" alt="code-example" style="zoom:50%;" />

```javascript
// 输出结果
sync code 1
sync code 2
i am async code
sync code 3
VM192:15 sto code
```

```JavaScript
console.log('1')
let p = new Promise((res, rej) => {
    console.log('放在这里还是同步代码')
    // 放在resolve或者sto这些里面就是异步的了
    res('3')
})
p.then(console.log)

console.log('2')

// !打印结果
// 1
// 放在这里还是同步代码
// 2
// 3
```

**①回调函数**

> 问题：回调地狱，嵌套层数深错误难以定位，但其实是一种同步代码。

**②generator**

> 对的你没看错只要在中间加 `*`就好了，看你是向左走还是向右走了 😨，官方示范是向左走。
>
> `yield`是生产的意思。
>
> 生成器函数的唯一之处是`promise`没有的**无穷流**（我暂时没搞明白）
>
> [自己实现惰性数据流、数据流操作符](https://segmentfault.com/a/1190000022135212)

```javascript
// 声明
// function *gen() {
// function * gen() {
// function* gen() {
function*gen() {
    console.log('a')
    yield "yield"
    console.log('b')
    yield "yield"
}

// 返回迭代器
const iterator = gen()
let str1 = iterator.next() // a
let str2 = iterator.next() // b
console.log(str1);// { value: 'yield', done: false }
console.log(str2);// { value: 'yield', done: false }
```

**③promise**

```text
解决回调地狱，但依旧是异步代码的格式

A+规范主干思路 1、Promise(回调函数) 回调函数是执行器（excutor），会执行resolve和reject两个函数

 1.1、出现错误就是reject【excutor错误、then的错误】

 2、then中传入的不是回调函数，需要二次封装

 3、then传入的状态是PENDING时需要依赖收集等【resolve/reject】执行

 4、then执行后必须返回新的Promise

 5、返回值一定是Promise，

 5.1普通值（含undefined）或者promise的FULFILLED状态

 5.2出现错误，或者promise的REJECTED状态
```

```javascript
// API
Promise.all()
args：promise数组（如果不是promise对象会被自动封装成promise对象），不按执行顺序，按参数传入顺序返回【需要全部都是resolve才能】
Promise.allSettled()
与上面相同，但是不会【错误中断】
Promise.any()
只要有一个是正确的就返回
Promise.then()
Promise.catch(callback) === Promise.then(null, ()=>{callback()})
Promise.finally()
Promise.race【竞速】()返回最先执行的那个
Promise.resolve() // 语法糖，直接跳过new，当你只需要一个方法时
Promise.reject()
```

```javascript
回调转promise
old: Func(opt, callback)

new:
    new Promise((resolve, reject)=>{
        Func(opt, _handleCallback(resolve, reject))
    })
  // 高阶函数封装Promise
  _handleCallback(resolve, reject) {
    // 返回回调函数
    return () => {
        if(){ resolve()}
        if(){ reject()}
    }
  }
```

**④async、await**

[扯扯 js 的async / await](https://blog.zsxsoft.com/post/21)

[用generator实现async+await](https://juejin.cn/post/6844904116368441351)

>  共生体，await语法只能再async函数内使用，将异步代码写成同步代码的格式，比起thenthenthen……更加优雅。
>
>  ​	但是promise比起 await和async的 try……catch用reject更加优雅。

```javascript
async function getValue () {
	let a = await axios.get() // 
    // 等待a返回才能执行
}
```

### 4、Date

>  时间日期API

```JavaScript
let time = Date.now() // 1970至今的毫秒数，用于计算gap，即执行间隔之类的懒函数或者节流函数
let time2 = new Date() // 标准时间，可以格式化
```

### 4、regexp

```javascript
函数
两个正则函数
    exec 返回 数组
            ["a", index: 0, input: "aa", groups: undefined]
            [正则, index, 字符串, 分组] 正则会携带部分信息，如下一次匹配的位置lastIndex
    test 返回 boolean
字符串函数
    match 非g模式下 返回 exec数组
    matchAll
    replace
    split

i 忽略大小写
g 全局
m 多行
. 所有字符包含空格
\w [a-zA-Z0-9_]
\d [0-9]
\s 所有空格

() 分组
| 或语法  (http|https)
[] 范围
[^0-9] 非数字
* 0到n
+ 1到n
? 0或1 还有贪婪的意思
{n,m} [n, m] {n,} [n, +∞]
^ 以什么开头
$ 以什么结尾

(?=【后缀】)
      eat(?=\scat)
      eat cat eat dog
      被匹配的内容是eat,cat前面的eat eat,dog不会被匹配到
(?<=【前缀】)
(?!【禁止含有此后缀】)
(?<!【禁止含有此前缀】)
```

### 5、闭包&立即执行函数

```javascript
闭包+立即执行函数的优点：只占用一个变量的命名空间，就可以进行丰富的操作
闭包：封闭独立的变量，函数的返回值是浅拷贝到执行空间去的
预编译：AO 形参、变量声明、实参赋值=>函数执行 GO 变量、函数声明=>执行
立即执行函数：可以进行复杂操作，然后销毁所有内部变量，变成一个返回值
执行期上下文（一个对象内容）：通过this取得的值
作用域：即变量的有效范围
作用域链：如闭包中的的子函数包含父函数的作用域 ，就近取

for(var i = 0; i < 5; i++){
    (function(i){
        setTimeout(()=>{
            console.log(i)
        },1000 * i)
    })(i)
}

function colsure(){
    let num = 1
    return function(){
        function add(){console.log(num++)}
        function sub(){console.log(num--)}
        return [add,sub]
    }
}

let colsureFunc = colsure()
let [add, sub] = colsureFunc()
add()
add()
sub()
add()
```

### 6、ajax

**原生xhr对象使用繁琐**

```javascript
1、实例化对象
const xhr = XMLHttpRequest()
2、open('GET','URL', false)// 第三个false表示阻塞，默认值是true
3、send()
4、监听
    xhr.onReadyStateChange = function(){
        xhr.readyState
            0    open() 前
            1    open() 后 send()前
            2    send() 后 响应前
            3    响应传输完成前
            4    响应全部传输完成
        xhr.status [200-299] 或 [304]
        xhr.response
    }
```

**fetch**

[fetch-with-mdn](https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch)

1、认为400、500响应码是正常的

~~2、不支持abort~~

3、信息不够丰富（没有progress）

4、默认不带cookie

```javascript
fetch('url').then((response) => {opt})
```

**axios**

>  拦截器是拦截每一次的`http`请求并做出处理

```javascript
1、支持node（httpproxy）、web
2、返回promise
3、最基本使用的方式:axios.get('url', ).then((response)=>{opt})

axios.interceptors.request.use(
	resolveCallback // 返回config，也就是回调函数的参数，实现链式调用
	, rejectCallback// return Promise.reject(error)
)
axios.interceptors.response.use(resolveCallback, rejectCallback)
```

### 7、跨域

[阮一峰的cors](https://www.ruanyifeng.com/blog/2016/04/cors.html)

[10 种跨域解决方案（附终极方案）](https://segmentfault.com/a/1190000022398875)

```javascript
跨域是指（协议、域名、端口）有任何一个不同，浏览器出于安全考虑禁止http传输
iframe + （domain | location.hash | name）、script、form等历史遗留的标签不存在这个问题
postMessage、websocket 等新出的api

jsonp：通过url发送get请求，通过query字符串确定变量名
cors（cross origin resource share）：跨域资源共享服务端设置Access（通道）
浏览器端自动处理cors、后台服务器需要自己设置响应头部
		简单请求：
        （1) 请求方法是以下三种方法之一：
            HEAD
            GET
            POST
        （2）HTTP的头信息不超出以下几种字段：
            Accept
            Accept-Language
            Content-Language
            Last-Event-ID
            Content-Type：只限于三个值application/x-www-form-urlencoded、multipart/form-data、text/plain
        复杂请求：
        res.setHeader("Control-Allow-Origin", url)
        res.setHeader("Control-Allow-Origin", *) // 无法携带cookie，可设置白名单对象
                      
开发模式：1、http proxy 2、nginx
```

**1、jsonp**

**客户端**

```javascript
<div id="jsonp"></div>
<script>
    function handle(data) {
        document.getElementById('jsonp').innerHTML = data.test;
    }
</script>
<script src="http://localhost:3000/jsonp-server?Func=handle"></script>
```

**服务端**

```javascript
const express = require('express');
const app = express();
app.listen(3000, () =>{
    console.log('http://localhost:3000')
});
app.get('/jsonp-server', (req, res) => {
    const func = req.query.Func
    let data = {
        test: 'i am a jsonp '
    }
    let ans = JSON.stringify(data);
    res.end(`${func}(${ans})`)
});
```

**2、cors**

**客户端**

```javascript
<button>send request</button>
<div id="ans"></div>
<script>
    const btn = document.querySelector('button');
    const ans = document.querySelector('#ans');
    btn.onclick = function () {
        const xhr = new XMLHttpRequest();
        xhr.open('GET', 'http://127.0.0.1:3000/cros-server?name=李四');
        xhr.send();
        xhr.onreadystatechange = function () {
            if (xhr.readyState === 4) {
                if (xhr.status >= 200 && xhr.status < 300) {
                    console.log(xhr.response);
                    ans.innerHTML = xhr.response;
                }
            }
        }
    }
</script>
```

**服务端**

```javascript
const express = require('express');
const app = express();
app.listen(3000);
const whiteList = {
    '张三': 'http://127.0.0.1:5501',
    '李四': 'http://127.0.0.1:9000',
}

app.get('/cros-server', (req, res) => {
    const name = req.query.name;
    if (whiteList.hasOwnProperty(name)) {
        res.setHeader('Access-Control-Allow-Origin', `${whiteList[name]}`);
    }
    res.setHeader('Access-Control-Allow-Headers', '*');// 允许全部header
    res.setHeader('Access-Control-Allow-Method', ['get', 'post', 'options']);
    res.send('hello cros')
});
```

### 8、this

```javascript
0、改变this指向的方式 call apply bind new
1、箭头函数没有this、他能获取到的(别人的)this是【不可修改的，在声明时就确定的】
2、
    2.1未实例化：
    非 `use strict` 时执行全局（window || global）， `use strict`就是undefined
    2.2实例化：
    指向实例化之后生成的对象
```

[下面的示范均来自这个回答](https://www.zhihu.com/question/353757734/answer/964557747)

**谁用就是谁**

```javascript
3、有狗
const foo = {
    bar: 10,
    fn: function() {
       console.log(this)
       console.log(this.bar)
    }
}
// 情况1【他还挂在foo上】
foo.fn()// foo 10
// 情况2【隐式挂载window上】// window.fn1()
var fn1 = foo.fn
fn1()// window undefined
console.log('-----------------------------------------')




const o1 = {
    text: 'o1',
    fn: function() {
        return this.text
    }
}
const o2 = {
    text: 'o2',
    fn: function() {
        return o1.fn()
    }
}
const o3 = {
    text: 'o3',
    fn: function() {
        var fn = o1.fn
        return fn() // 没有.语法即是window
    }
}

console.log(o1.fn())// o1
console.log(o2.fn())// o1
console.log(o3.fn())// undefined 识别全局，但是全局下没有声明text变量
global.text = 'window'
console.log(o3.fn())// 'window'，global是node环境的全局对象
// call 改this并执行， apply改this并执行
// bind 改this返回新函数

// 让o2输出指定的o2
const o1 = {
    text: 'o1',
    fn: function() {
        return this.text
    }
}
const o2 = {
    text: 'o2',
    fn: o1.fn
}

console.log(o2.fn())// o2
```

**离得近**

```javascript
// 离得近
const person = {
    name: 'Lucas',
    brother: {
        name: 'Mike',
        fn: function() {
            return this.name
        }
    }
}
console.log(person.brother.fn()) // 'Mike'，brother离得近，就执行brother
```

**改变this的优先级**

**结论：new &gt; call**

```javascript
// call 这种显示改变的比默认绑定到全局的优先级高
function foo (a) {
    console.log(this.a)
}

const obj1 = {
    a: 1,
    foo: foo
}

const obj2 = {
    a: 2,
    foo: foo
}

obj1.foo.call(obj2)// 2
obj2.foo.call(obj1)// 1
console.log('-----------------------------------------')


// 其实我感觉又一点问题，只能说是new能覆盖call的修改，并不能直接说明优先级的问题
function foo (a) {
    this.a = a
}

const obj1 = {}
// 情况1
var bar = foo.bind(obj1)
bar(2)// obj1.foo(2)
console.log(obj1.a)// 2
// 情况2
var baz = new bar(3)
console.log(baz.a)// 3
```

**箭头函数尝试改变this**

```javascript
function foo() {
    return a => {
        console.log(this.a)
    };
}

const obj1 = {
    a: 2
}

const obj2 = {
    a: 3
}

// obj1.foo()
const bar = foo.call(obj1)//  2
// 试图修改箭头函数（bar）this失败
console.log(bar.call(obj2))// undefined
```



**上面的示范很不优雅我修改了一下**

>  ​	最终的结论是：箭头函数没有 `this`但是可以使用 `this`且是无法修改的，是声明是就确定对父级作用域的引用，但是父级作用域的`this`发生了改变箭头函数的`this`也会发生改变

```javascript
function foo(a) {
    this.a = a
    return a => {// 此处的a跟this没有任何关系
        console.log(this.a)
    };
}

const obj1 = {
    a: 2
}

let arrow = new foo(4)
arrow.call(obj1)// 4，即箭头函数的this指向无法修改
console.log(arrow)// [Function (anonymous)]
```

```javascript
function foo(a) {
    console.log(this.a)
    return a => {// 此处的a跟this没有任何关系
        console.log(this.a)
    };
}

const obj1 = {
    a: 2
}
let arrow = foo.call(obj1)// 2
arrow()// 2
```



```javascript
const a = 123 // 不会挂载window上
// global.a = 123 // 即使这里是global的下面也是undefined，所以容易引起歧义
const foo = () => a => {
    console.log(this.a)
}

const obj1 = {
    a: 2
}

const obj2 = {
    a: 3
}

var bar = foo.call(obj1)// undefined，这里只是函数的返回值还是函数所以是undefined
console.log(bar) //[Function (anonymous)]
console.log(bar.call(obj2))// undefined
```

### 9、function

```javascript
默认返回值是undefined
其他时候返回this
let c = 0
fuction add (a, b) {
    c = a + b
}
let res = add(1, 2)
console.log(c) //  3
console.log(res) // undefined
```

#### 9.1、call、apply、bind

> 使用场景，如vue2中的对象需要获取数组方法，即给伪数组添加数组方法
> 并且在超出预期的地方修改`this`，如`dom`的事件指向的`this`非预期。

都是挂载在构造函数`Function`的原型上，即 `Function.prototype`

|  | call | apply | bind |
| :--- | :--- | :--- | :--- |
| 功能 | 都是改变 `this` 指向 |  |  |
| 参数 | `(obj, ...agrs)` | `(obj, [])` | `(obj, ...agrs)` |
| 执行 | 立即执行 | 立即执行 | 返回新的函数，可以二次传入参数再执行 |
| 场景 | 实现原型链继承 |  | `addEventListener()`等不需要立即执行的函数，不写bind就会绑定到`node`上，而不是我们需要的`this`上。 `this.oBtn.addEventListener('click', this.btnClick.bind(this), false);` |
| 位置 | `Function.prototype` |  |  |

```javascript
function show(...args) {
    console.log(args);
    console.log(this.name);
}

Function.prototype.rCall = function (ctx, ...args) {
    let fn = Symbol(1);
    // 将方法挂载ctx上,this指向的rCall函数的调用者
    ctx[fn] = this;
    // 执行挂载的方法
    ctx[fn](...args);
    // 执行完成之后删除原方法
    delete ctx[fn];
}

show.rCall({ name: 'rCall' }, 'args1', 'args2')

Function.prototype.rApply = function (ctx, arr = []) {
    let fn = Symbol(1);
    if (arr && !(arr instanceof Array)) {
        throw ('arguments[1] not a Array');
    }
    ctx[fn] = this;
    ctx[fn](...arr);
    delete ctx[fn];
}
show.rApply({ name: 'rApply' }, ['args1', 'args2'])

Function.prototype.rBind = function (ctx, ...args1) {
    let fn = Symbol(1);
    return (...args2) => {
        ctx[fn] = this;
        ctx[fn](...args1.concat(args2))
        delete ctx[fn];
    }
}

let obind = show.rBind({ name: 'rBind' }, 'args1', 'args2')
obind('args3')
```

### 10、技巧

#### 10.1、鬼一样的循环和分支

```javascript
for(const value in Obj){} // 不能保证顺序，用于遍历数组不可靠
for(const value of xx){} // 能保证顺序，但是需要实现(迭代器)接口
xx 可以是 string Array Map weakMap Set weakSet
// 还有forEach和map就不介绍了

while(){} // 容易写出不确定次数的循环
for(){} // 容易写出确定次数的循环
do{}while()  // 至少执行一次
do {
    console.log('false can run');
} while (false)

let action = {
    type: "run"
}

if() else if(){} else{} // 常用写法

switch (action.type) {// 适用于条件类型相同且条件十分多的情况
    case "run":
        console.log('run');
        break;// 关闭break 会一直往下执行
    case "go":
        console.log('go');
        break;
    default: // 前面的case都没有执行时就会执行
        console.log('no run');
        break;
}

return isObject() ? 'this is a obj' : 'not a obj' // 放在返回值好用
```

#### 10.2、异常处理

```javascript
try {
    // 可能出现错误的代码块
} catch(e) {
    throw new Error(e) // 抛出错误信息，中断程序
    // e是本来不处理就直接中断程序并打印到控制台的信息
} finally { // 管你出不出错一定执行
    // 一定会被执行到的代码块（即不受前面的代码影响）
}
```

#### 10.3、with改变作用域

```javascript
const obj = {
    name: "zoulam",
    age: 18
}
with (obj) {
    console.log(name);// "zoulam"
    console.log(age);// 18
}
```

#### 10.4、柯里化（curry ）

[柯里化介绍](https://github.com/mqyqingfeng/Blog/issues/42)

> 注意不要忘记`apply`的第二个参数是数组

```javascript
function curry(func) {
  return function curried(...args) {
    // 关键知识点：function.length 用来获取函数的形参个数
    // 补充：arguments.length 获取的是实参个数
    if (args.length >= func.length) {
      return func.apply(this, args)
    }
    return function (...args2) {
      return curried.apply(this, args.concat(args2))
    }
  }
}

// 测试
function sum (a, b, c) {
  return a + b + c
}
const curriedSum = curry(sum)
console.log(curriedSum(1, 2, 3))
console.log(curriedSum(1)(2,3))
console.log(curriedSum(1)(2)(3))
```

<img src="https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201126011007411.png" alt="curry" style="zoom:50%;" />

```javascript
// 奇怪的api
console.log(mutli(1).val) // 1
console.log(mutli(1)(2).val) // 5
console.log(mutli(1)(2)(3).val) // 14


// 思路分析：
// 链式调用，返回值是函数
// 子函数上挂着val


function mutli(init) {
    getVal.val = 0
    function getVal(val) {
        getVal.val += Math.pow(val, 2)
        return getVal
    }
    getVal(init)
    return getVal
}
```

#### 10.5、compose（柯里化进阶，React的高阶组件）

**组合函数按从右往左顺序执行，右边的返回值是左边的参数，输入值是函数，返回值也是函数**

```javascript
function func1(arg) {
    console.log('func1', arg);
    return arg + 1;
}

function func2(arg) {
    console.log('func2', arg);
    return arg + 1;
}

function func3(arg) {
    console.log('func3', arg);
    return arg + 1;
}


// 一直向外叠加函数，但是始终不执行
// function compose(...funcs) {
//     if (!funcs.length) return arg => arg;
//     if (funcs.length === 1) return funcs[0];
//     // reduce(arg1, arg2) arg1 上一次的返回值，args数组当前值
//     return funcs.reduce((lastReturn, currentFunc) => {
//         // [Function: func3] [Function: func2]
//         // [Function] [Function: func1]
//         console.log(lastReturn, currentFunc)
//         // ()=>{} 就是最终返回的函数,但是都没有执行
//         return (...args) => {
//             lastReturn(currentFunc(...args))
//             console.log(args);// [1] [2]
//         }
//     })
// }

function compose(...funcs) {
    if (!funcs.length) return arg => arg
    if (funcs.length === 1) return funcs[0]
    return funcs.reduce((func2, func1) =>
        (...args) => {
            func2(func1(...args))
        }
    )
}

// 旧
func3(func2(func1(0)))
console.log('---------------------------------------------------------------------------');
// 新
compose(func3, func2, func1)(0);
// 装饰器
@a
@b
@c
```

#### 10.6、**解构**

```javascript
// 1、更新对象（下面示范对象的，数组同理，但是数组有自己的方法）
oldObject，newItem

oldObject = {...oldObject, [id]: newItem}

function MyComponent({state, props, ...rest}){}

//  2、数组去头
// 下面的newArr就是去除原数组arr第一项之后的新数组
let arr = [1, 2, 3, 4, 5]
let [, ...newArr] = arr
console.log(newArr)// [2, 3, 4, 5]
```

#### 10.7、偷方法

```javascript
let obj = {length:0}
Array.prototype.[func].call(obj, ...args)
// obj从数组的原型上获取了func方法




// Vue中重建原型链
const oldProto = Array.prototype
const proto = Object.create(oldProto) // 这里需要分号

['shift', 'push'].forEach((method) => {
    proto[method] = function () {
        // 此处添加功能,Vue在这里触发更新视图
        oldProto[method].call(this, ...arguments)
    }
})
let arr = [1, 2, 3]
Object.setPrototypeOf(arr, proto)
```

#### 10.8、二维数组

```javascript
// 创建5 * 5的二维数组,并初始化数组元素为0
let row = 5
let col = 5
let doubleArr = Array.from({ length: row }, () => new Array(col).fill(0))
```

#### 10.9、数组和对象

> **注:** `Array.form()`也可以格式化`Set`、等可迭代对象

```javascript
// 对象 => 数组
1、Array.form(obj) // 含有length属性的对象

2、Object.keys(obj).map(key => obj[key]);
```

```javascript
// 数组变对象
const flattenArr = (arr) => {
    return arr.reduce((map, item) => {
        map[item.id] = item;
        return map
    }, {})
}
```

#### class和传统构造函数的对比

```JavaScript
class B {
    constructor(val) {
        this.val = val
    }
}


class A extends B {
    state = 0 // 这个也是this
    constructor(val) {
        super(val)
        this.props = 0
    }
    static Fn() {
        console.log('in prototype')
    }
}

let a = new A('11')
console.log(a.state) // 0
A.Fn() // in prototype
console.log(a.val) // 11
// 静态方法(旧版)
function C(){}
C.value
// 静态方法
class C {static value}
// super向父类传值，挂载到this上就可以轻松获取，在Java中父类存储公共方法，和挂载需要的属性
```

### 11、花式继承

[六种继承方式](https://segmentfault.com/a/1190000016708006)

>  下面的演示中，`super`是父构造函数，`sub`是子构造函数

| 继承方式       | 实现方式                               | 缺陷                                                         |
| -------------- | -------------------------------------- | ------------------------------------------------------------ |
| 原型链继承     | `sub.prototype= new super()`           | 内部引用值的修改会被永久修改，同时不能给父类传参。           |
| 盗用构造函数   | `function sub(){super.call(this)}`     | 方法只能写在构造函数内，无法复用。                           |
| 组合继承       | 用原型链继承方法，盗用构造函数继承属性 | 最主要的效率问题就是父类构造函数始终会被调用两次：一次在是创建子类原型时调用，另一次是在子类构造函数中调用。 |
| 原型式继承     |                                        |                                                              |
| 寄生式继承     | 克隆 `super`对象赋值给`sub`对象        | 通过寄生式继承给对象添加函数会导致函数难以重用，与构造函数模式类似。 |
| 寄生式组合继承 | 见下方代码                             | 解决前面的缺陷，使用副本的方式完成继承                       |

```javascript
----------------------组合继承----------------------------
let inherit = (function () {
    let Buffer = function () { };
    return function (Target, Origin) {
        Buffer.prototype = Origin.prototype;
        Target.prototype = new Buffer();
        Target.prototype.constructor = Target;
        Target.prototype.super_class = Origin;
    }
})();
```

### 12、EventLoop【输出问题】

>  结论：`sync（同步代码） > microtask(process.nextTick > promise.then) > macrotask(setTimeout)`

[Event Loop的规范和实现](https://zhuanlan.zhihu.com/p/33087629)

<img src="https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/1053223-20180831162350437-143973108.png" alt="EventLoop" style="zoom:50%;" />

> 微任务和宏任务是异步任务的分类

**微任务**\(microtask\)

| \# | 浏览器 | Node |
| :--- | :--- | :--- |
| `process.nextTick` | x | √ |
| `MutationObserver` | √ | x |
| `Promise.then catch` | √ | √ |

**宏任务** \(macrotask\)

| \# | 浏览器 | Node |
| :--- | :--- | :--- |
| `let id = setTimeout(callback, time)`返回id用于清除副作用 | √ | √ |
| `setInterval(callback, time)` 返回id用于清除副作用 | √ | √ |
| `setImmediate` | x | √ |
| `requestAnimationFrame` | √ | x |

```javascript
// -----------注意此处的两setTimeout的时间不一样--------------
// -------------------一致则是前面的先执行-------------------
var p = new Promise(resolve => {
    setTimeout(() => {
        console.log('Promise sto',7);
    },1000)
    console.log('normal Promise', 4);
    resolve(5);
});

function func1() {
    console.log('func1', 1);
}

function func2() {
    setTimeout(() => {
        console.log('sto', 2);
    },100);
    func1();
    console.log('func2', 3);
    p.then(value => {
        console.log('then1',value);
    }).then(() => {
            console.log('then2',6);
        });
}

func2();
```

```javascript
// 三个同步代码顺序执行
normal Promise 4
func1 1
func2 3
// 两个微任务先执行
then1 5
then2 6
// 宏任务看计时器执行
sto 2
Promise sto 7
```

### 13、[Web Workers](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Workers_API/Using_web_workers)

> 用于创建后台线程

### 14、[Service Worker](https://developer.mozilla.org/zh-CN/docs/Web/API/Service_Worker_API)

> Service workers 本质上充当 Web 应用程序、浏览器与网络（可用时）之间的代理服务器。

### 15、[websocket](https://developer.mozilla.org/zh-CN/docs/Web/API/WebSocket)

> 全双工的 通信api，用于实时聊天等场景，比起`http2.0`的长连接有着服务端主动发送消息的优势。

### 16、鬼一样的模块化

[commonjs 与 esm 的区别](https://juejin.cn/post/6844903861166014478)

#### 16.1、ES6Module（esm）

> 优势：
>
> 1、浏览器支持，node通过传入参数（传入参数的方式会让**require**函数失效）也可以直接使用（也可以通过babel转义）
>
> node支持方式
>
> ​	①package.json 传入 `"type" :"module"`
>
> ​	②文件名改为`mjs`，导入也需要修改
>
> ​	③执行时传入 ` --experimental-modules`
>
> 2、ES6 模块**不是对象**，而是通过`export`命令显式指定输出的代码，再通过`import`命令输入，
>
> 被称为**【“编译时加载”或者静态加载】**是一种按需导入模式，比起`cjs`全部导出成对象要高效。
>
> 3、特征：
>
> ​	①**动态引用**，就是引入的模块发生的变化导入的也变化（引入模块`settimeout`，然后再 `settimeout`打印即可观察 ），这是**commonjs**不具备的
>
> ​	②不可变，无法修改（但是引用值的内部可修改），与const类似
>
> 语法：
>
> 不可以省略`.js`的文件类型
>
> `export` 导出对象或者元素，`export default`默认导出挂载到导出对象的 `default`上
>
> `import`导入 ，`*`引入所有，`as`重命名，`{default as xx}` 重命名默认

![error](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201126190153011.png)

```javascript
 <script type="module" src='./src/app.js'>


// path:src/js/a.js
const React = {
    Component: Component
}

function Component() { }
const b = 'b module test'
// 声明导出
export const a = 'a module test'
// 统一导出
export { Component, b }
// 默认导出
export default React


// path: /src/app1.js
import TestReact, { Component, a as testA, b as testB } from './js/a.js'
// 直接对 默认导出重命名为TestReact 使用as重命名，default可以不用使用as语法直接重命名
console.log(TestReact);
console.log(testA);
console.log(testB);

// path: /src/app2.js
import * as AModule from './js/a.js'
const { a, b: TestB, Component } = AModule// 使用解构赋值重命名
console.log(a);
console.log(TestB);
console.log(Component);
console.log(AModule);
console.log(Object.prototype.toString.call(AModule)); //app2.js:11 [object Module]

// path: /src/app3.js
import { default as TestReact } from './js/a.js'
console.log(TestReact);
```

#### 16.2、CommonJS\(cjs\)

> `node.js` 支持，拷贝引用，即后续发生的变化也不会变化
>
> 通过全局函数 `require` 实现，
>
> ```javascript
> module wrapper function包装函数
> (function (exports, require, module, __filename, __dirname）{
>            
> })
> ```
>

`module.exports`

```javascript
// path：src/js/a.js
function A() {

}
function B() {

}
module.exports = {
    A,
    B
}

// path:src/app.js
// 可以省略js文件类型
const a = require('./js/a')
//可以直接使用解构赋值
const { B } = require('./js/a')
const A = require('./js/a').A
console.log(a);
console.log(Object.prototype.toString.call(a));//[object Object]
console.log(B);
console.log(A);
```

`exports`（不推荐）

> 隐式在头部声明 `var exports = module.exports`，即两者不能并存，存在覆盖关系，推荐上面的对象导出方式。

```javascript
// path：src/js/a.js
exports.A = function () {

}
exports.B = function () {

}

// path:src/app.js
// 可以省略js文件类型
const a = require('./js/a')
//可以直接使用解构赋值
const { B } = require('./js/a')
const A = require('./js/a').A
console.log(a);
console.log(Object.prototype.toString.call(a));//[object Object]
console.log(B);
console.log(A);
```

### 17、JSON

> JavaScript Object Notation ，可以理解为JavaScript对象格式的数据， [MDN-JSON](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/JSON)
>
> **注：**手写json格式 **字符串** 的时候不能忘记在`key`中加上引号，场景 `postman`和 `data-*`,

```javascript
支持数组、对象、普通值（数字、字符串、boolean、null）
以前不支持 undefined、变量、函数、对象实例（会被过滤掉，变量和对象实例会被认为是普通值）
```

```javascript
let a = { name: "lala" }
class Baby {
    age = 18
}
let b = {
    a: a,
    no: undefined,
    sayName: function () {
        console.log(this.a.name)
    },
    baby: new Baby()
}

let str = JSON.stringify(b)// 第二参数是替换器函数，第三个参数是Number类型的缩进
// {"a":{"name":"lala"},"baby":{"age":18}}
console.log(str)
let obj = JSON.parse(str, cb) // 第二个参数用于筛选的回调函数
console.log(obj)
```

### 18、[标准库](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects)

#### [File对象](https://developer.mozilla.org/zh-CN/docs/Web/API/File)

```javascript
let myFile = new File(bits, name[, options]);
```

#### [Image对象](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLImageElement/Image)

```javascript
let oImg = new Image(100, 200)// 参数1 是iamge的width 参数2是image的height
oImg.src = '../static/girl.jpg'
document.body.appendChild(oImg)
```

#### [Blob对象](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob)

>  `binary large object` 巨型的二进制对象

```text

```

#### [requestIdleCallback](https://developer.mozilla.org/zh-TW/docs/Web/API/Window/requestIdleCallback) 

注册浏览器进程空闲时间的函数，ReactFiber架构就实现了他的polyfill

```JavaScript
requestIdleCallback(workLoop)
workLoop(deadline){
	deadline.timeRemaining()// 注入参数，并且上面挂载着可以获取当前轮空闲时间单位是ms
}
```

#### RAF

> [requestAnimationFrame MDN文档](https://developer.mozilla.org/zh-CN/docs/Web/API/window/requestAnimationFrame)

```JavaScript
// 传入回调函数
```

#### [FormData构造函数/类](https://developer.mozilla.org/zh-CN/docs/Web/API/FormData)

> 表单数据对象，存储表单数据信息
>

```javascript
let fd  = new FormData()
fd.append(key, value)// 添加
console.log(fd)
fd.get(key) // 输出value
fd.append(key, value)// 设置
fd.has(key)// return boolean
fd.delete(key)// 删除后变成null 而不是undefined
```

#### [localStorage](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/localStorage)

>  同源访问，永久存储（用户不手动清空的情况下）

```javascript
localStorage.setItem(key, value)
localStorage.getItem(key)
localStorage.reomoveItem(key)
localStorage.clear() // 清空
```

#### [sessionStorage](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/sessionStorage)

> 只对当前页面生效（同一个网站，浏览器打开两个也是不一样的），页面关闭自动销毁。

```
sessionStorage.setItem(key, value)
sessionStorage.getItem(key)
sessionStorage.reomoveItem(key)
sessionStorage.clear() // 清空
```

# 参考文章

[浏览器的工作原理：新式网络浏览器幕后揭秘](https://www.html5rocks.com/zh/tutorials/internals/howbrowserswork/)

[回流（reflow）和重绘（Painting）](https://zhuanlan.zhihu.com/p/52076790)

[触发重绘的属性列表](https://gist.github.com/paulirish/5d52fb081b3570c81e3a)

[浏览器工作原理：从输入URL到页面加载完成](https://github.com/amandakelake/blog/issues/55)

[30分钟学会flex](https://zhuanlan.zhihu.com/p/25303493)

[那些你总是记不住但又总是要用的css](https://zhuanlan.zhihu.com/p/231014167)

[你需掌握的CSS知识都在这了](https://zhuanlan.zhihu.com/p/231014167)

[相对单位在线转换网站](http://pxtoem.com/)

[去除浏览器默认样式，并初始化固定样式代码获取](https://github.com/necolas/normalize.css) 

[塞贝尔曲线可视化](https://cubic-bezier.com/)

# API文档

[H5新增内容](https://developer.mozilla.org/zh-CN/docs/Web/Guide/HTML/HTML5)

 [map标签](https://www.cnblogs.com/okgoodman/p/8586524.html)

[更多伪类](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes)