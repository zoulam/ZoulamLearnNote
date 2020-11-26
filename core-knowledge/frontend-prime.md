# 前端入门

## 浏览器

[浏览器的工作原理：新式网络浏览器幕后揭秘](https://www.html5rocks.com/zh/tutorials/internals/howbrowserswork/)

[回流（reflow）和重绘（Painting）](https://zhuanlan.zhihu.com/p/52076790)

[出发重绘的属性列表](https://gist.github.com/paulirish/5d52fb081b3570c81e3a)

渲染过程

![webkit](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/webkitflow.png)

合成

![&#x5C06; DOM &#x4E0E; CSSOM &#x5408;&#x5E76;&#x4EE5;&#x5F62;&#x6210;&#x6E32;&#x67D3;&#x6811;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/render-tree-construction.png)

1. 解析HTML，生成DOM树，解析CSS，生成CSSOM树（取决于你在html文档的顺序）
2. 将DOM树和CSSOM树结合，生成渲染树\(Render Tree\)
3. Layout/reflow\(回流\):根据生成的渲染树，进行回流\(Layout\)，得到节点的几何信息（位置，大小）
4. Painting\(重绘\):根据渲染树以及回流得到的几何信息，得到节点的绝对像素
5. Display（显示）:将像素发送给GPU，展示在页面上。（这一步其实还有很多内容，比如会在GPU将多个合成层合并为同一个层，并展示在页面中。而css3硬件加速的原理则是新建合成层，这里我们不展开，之后有机会会写一篇博客

【visibility和opacity隐藏的是可见节点，**display:none是不可见节点，不再dom结构中**】

**reflow**：可见节点和其样式结合的再**与设备的viewport**计算出准确单位`px`的过程

**painting**：节点的位置信息发生改变，新增，删除，修改【尺寸】，等等

消耗性能的原因：浏览器【**现代浏览器是达到临界值**】会在绘制过程中**清空队列重新绘制**

**回流一定会触发重绘，而重绘不一定会回流**

### 优化

1、合并js的样式修改，先创建样式文本再**追加**到 `style.cssText+=` 上

2、批量修改DOM，①脱离文档流②进行复杂修改③回到文档流

2.1、使用文档碎片（createDocumentFragment）的方式创建节点，全部添加到Fragment完成后再添加到页面

2.2、`display:none` =&gt;更新节点内容=&gt;`display:block`

3、缓存值相同值

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

defer 延迟，**HTML5规范要求**，两个defer保证顺序 保证在DOMContentloaded之前，**但事实并非如此**。

async 异步【下载不会阻塞，用于存放不修改DOM的代码】，两个异步无先后顺序， onload之前，DOMContentloaded**之前或之后**

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

src :script source img input(image) iframe
href:link a

// 行元素 
// 不占满一行，内容填充，不可设宽高
a b span img【可替换元素，虽然是行元素但是能设置宽高】
strong sub sup button input label select textarea
// 块元素
// 可设宽高，独占一行
div ul【无序列表】 ol【有序列表】 li【列表元素】 
dl【定义列表】 dl的列表元素：（dt dd） h1 h2 h3 h4 h5 h6 p 

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
 table: thead tbody tfoot
                 tr(行：table row)
                     td（列）(colspan="2"向下合并2行、rowspan="2"向右合并2列)
/* 指定边框之间的距离 */
border-spacing: 0px;
/* 设置边框的合并 */
border-collapse: collapse;
```

## CSS

[30分钟学会flex](https://zhuanlan.zhihu.com/p/25303493)

[那些你总是记不住但又总是要用的css](https://zhuanlan.zhihu.com/p/231014167)

[你需掌握的CSS知识都在这了](https://zhuanlan.zhihu.com/p/231014167)

[相对单位在线转换网站](http://pxtoem.com/)

```javascript
link 是 html标签，除了css还能引入图标，顺序加载，可以使用JS的DOM操作
@import是css模块化语法，页面加载完毕后加载

选择器
    . # div * 
    不用逗号隔开表示同时满足 a.b（选择b）
    a,b 或（两个元素都选择）
    a > b（父子） a b(祖孙含父子)
    a+b(第一个兄弟) a~span（后面的全部兄弟span，不含a）
    [title] [title=abc] 与正则语法类似 
    正则选择器（性能较差慎用）
    [titile^=abc] [title$=abc] [title*=abc]
伪类(:) pseudo-class
    //    就像添加了特定的类名
    a link visited hover active 【必须按顺序设置存在覆盖关系】
    :first-child :last-child 
    :nth:child(n) :nth:child(2n)偶数 :nth:child(2n+1)奇数 :nth:child(3n)3的倍数
    :p:not(.pointer) {} 选择类名不是pointer的
伪元素(::) pseudo-element
    // 就像是添加了特定元素节点
    selection 被选中文本 first-line first-letter首字母
    after{content: '』';} before{content: '『';}

优先级
    !important > inline > id > class/pseudo class > element > * > inherit

盒模型
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

可继承的属性【不相记，当样式不同于预期时需要考虑】

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
    rem(html min=12px) em【父元素的font-size】 px 
    以iphone6的375px *667px为例
    vw【100vw=375px】 vh【100vw=667px】 deg（旋转的属性的单位）
    rgb rgba colorName
    不再缩放出现滚动条：min-width min-height
    calc(100px/2) 50px

常用属性
    list-style
    cursor
    pointer-events
    opacity:0 【透明】

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
    -size
    line-height (行间距 == line-height - font-size)
    family
    font-weight 字体粗细

font-style
    text-decoration (装饰)
    text-align【行元素水平方向排列，相对父元素的排列方式】
        justify文字向两侧对齐
    vertical-align【行元素，相对父元素垂直方向的排列方式】
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

<img src="https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201124172104259.png" alt="died area" style="zoom:50%;" />

```javascript
声明方式
var(变量提升、全局作用域) 
let(块级作用域,暂时性死区) 
const（必须初始化，暂时性死区，基本数据类型不可修改，引用数据类型的元素可修改）

基本数据类型（必须小写、不然会与包装类重名）
number（包含以下内容）：
(float int Infinity NaN 【-2 ** 53 ~ 2 ** 53 -1】【1e6 == 1000000】 1e9 + 7) 
string undefined null bigInt symbol boolean
bigInt声明方式 BigInt(15) 或 15n ， 
Symbol使用方式 Symbol(1) console.log(Symbol(15) == Symbol(15));// false
包装类：Number Boolean String，存在包装类，这三个也能使用.function的语法

真值：
    number （非0&&非NaN）
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
if([] == []) false
if({} == {}) false
if([] == false) true 
// 对引用值的赋值拷贝都是浅拷贝，后续修改会修改到原来的值
let obj1 = { name: 'zoulam' }
let obj2 = obj1
obj2.name = 'lala'
console.log(obj1, obj2)//{ name: 'lala' } { name: 'lala' }



判断方式
typeof ---- 小写 function(){}:(function) null:(object) 基本数据类型
Object.prototype.toString.call()---- [object 大写] 精准
instanceof ---- 返回boolean，任何数据instanceof Object都是true
constructor ---- 获取构造器 [Function xx]

常用代码段
typeof xx == "object" && xx !== null

隐式类型转换（除了字符串拼接都是转化为数字）
+ 结果一定是 string类型
(非加号运算符) - * / %        string => number(NaN)
(正号/负号)+a -a             string => number(NaN)
(自减、自加/减等于、加等于) a++ a-- --a ++a a += 1 a -=1 string => number(NaN)
(比较运算符> < == !==) 字符串转化为ASCII码 从左到右（带数量级的比较）string => number

(位运算 & | ~ >> << ^ >>>) string => number
& 全1为1          全是1  就是1
| 一个1就是1        有1   就是1
^ 按位异或 跟|相反  0或1  就是1 【严格或】
~ 按位非 
    ~a == -(a+1) ~15 == -16
<< * 2
>> / 2 向下 3 >> 1 == 1
>>> 正数 * 2 负数会变成超大正数(32位数字)

(【短路】逻辑运算符 || && !) 返回值是中断值（达成true条件）
, 从左到右

Number(num) 不断尾巴,直接NaN
parseInt(num, radix) 断尾
parseFloat(num) .toFixed() .toPrecision()

undefined == null true 其他为false
```

### 2、proto

`prototype`是函数特有结构，`__proto__`是对象属性是原型链的链条

```javascript
function myNew(Func, ...args) {
    let instance = {}
    Object.setPrototypeOf(instance, Func.prototype)
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

### 3、异步编程

**①回调函数**

**②generator**

```javascript
// 声明
function * gen(){
    console.log('a')
    yield "a"
    console.log('b')
    yield "b"
}

// 返回迭代器
const iterator = gen()
iterator.next()
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
API
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
Promise.resolve()
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
闭包和立即执行函数的优点：只占用一个变量的命名空间，就可以进行丰富的操作
闭包：封闭独立的变量
预编译：AO 形参变量声明、实参赋值=>函数执行 GO 变量函数声明=>执行
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

```text
1、实例化对象
2、open('GET','URL')
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

1、认为400、500响应码是正常的

~~2、不支持abort~~

3、信息不够丰富（progress）

4、默认不带cookie

```javascript
fetch('url').then((response) => {opt})
```

**axios**

```text
1、支持node（httpproxy）、web
2、返回promise
3、最基本使用的方式:axios.get('url', ).then((response)=>{opt})
```

### 7、跨域

```javascript
跨域是指（协议、域名、端口）有任何一个不同，浏览器出于安全考虑禁止http传输
iframe、script、form等历史遗留的标签不存在这个问题
jsonp：通过url发送get请求，通过query字符串确定变量名
cors：跨域资源共享服务端设置Access（通道）
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
app.listen(3000);
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
    res.setHeader('Access-Control-Allow-Headers', '*');
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
情况1【他还挂在foo上】
foo.fn()// foo 10
情况2【隐式挂载window上】// window.fn1()
var fn1 = foo.fn
fn1()// window 10
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
        return fn()
    }
}

console.log(o1.fn())// o1
console.log(o2.fn())// o1
console.log(o3.fn())// undefined 识别全局，但是全局下没有声明text变量

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
离得近
const person = {
    name: 'Lucas',
    brother: {
        name: 'Mike',
        fn: function() {
            return this.name
        }
    }
}
console.log(person.brother.fn()) // 'Mike'
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


function foo (a) {
    this.a = a
}

const obj1 = {}

// 情况1
var bar = foo.bind(obj1)
bar(2)// obj1.foo(2)
console.log(obj1.a)// a
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

```javascript
const a = 123 // 不会挂载window上
const foo = () => a => {
    console.log(this.a)
}

const obj1 = {
    a: 2
}

const obj2 = {
    a: 3
}

var bar = foo.call(obj1)// undefined
console.log(bar.call(obj2))// undefined
```

### 9、function

```text
默认返回值是undefined
其他时候返回this
```

#### call、apply、bind

`Array.map(a, b)`   等价于 `map.call(Array, a, b)`；

都是挂载在构造函数`Function`的原型上，即 `Function.prototype`

|      | call                 | apply       | bind                                       |
| ---- | -------------------- | ----------- | ------------------------------------------ |
| 功能 | 都是改变 `this` 指向 |             |                                            |
| 参数 | `(obj, ...agrs)`     | `(obj, [])` | `(obj, ...agrs)`                           |
| 执行 | 立即执行             | 立即执行    | 返回新的函数，可以二次传入参数再执行       |
| 场景 | 实现原型链继承       |             | `addEventListener()`等不需要立即执行的函数 |
| 位置 | `Function.prototype` |             |                                            |

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

#### 鬼一样的循环和分支

```javascript
for(const value in Obj){} // 不能保证顺序，用于遍历数组不可靠
for(const value of Obj){} // 能保证顺序
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

#### with改变作用域

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



#### 柯里化（curry ）

[柯里化介绍](https://github.com/mqyqingfeng/Blog/issues/42)

> ​	注意不要忘记`apply`的第二个参数是数组

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

![curry](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201126011007411.png)

#### compose（柯里化进阶，React的高阶组件）

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

#### **解构**

```javascript
1、更新对象
oldObject，newItem

oldObject = {...oldObject, [id]: newItem}

function MyComponent({state, props, ...rest}){}

2、数组去头
// 下面的newArr就是去除原数组arr第一项之后的新数组
let [, ...newArr] = arr
```

#### 偷方法

```javascript
let obj = {length:0}
Array.prototype.[func].call(obj, ...args)
// obj从数组的原型上获取了func方法
```

#### 二维数组

```javascript
// 创建5 * 5的二维数组
let doubleArr = Array.from({ length: 5 }, () => new Array(5))
```

#### 数组和对象

> ​	**注:** `Array.form()`也可以格式化`Set`、等可迭代对象

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



### 11、花式继承

[六种继承方式](https://segmentfault.com/a/1190000016708006)

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

![EventLoop](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/1053223-20180831162350437-143973108.png)

> 微任务和宏任务是异步任务的分类

**微任务**\(microtask\)

| \# | 浏览器 | Node |
| :--- | :--- | :--- |
| `process.nextTick` | x | √ |
| `MutationObserver` | √ | x |
| `Promise.then catch finally` | √ | √ |

**宏任务** \(macrotask\)

| \# | 浏览器 | Node |
| :--- | :--- | :--- |
| `setTimeout(callback, time)`返回id用于清除副作用 | √ | √ |
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
// normal Promise 4
// func1 1
// func2 3
// then1 5
// then2 6
// sto 2
// Promise sto 7
```

### 13、[Web Workers](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Workers_API/Using_web_workers)

> 用于创建后台线程

### 14、[Service Worker](https://developer.mozilla.org/zh-CN/docs/Web/API/Service_Worker_API)

> Service workers 本质上充当 Web 应用程序、浏览器与网络（可用时）之间的代理服务器。

### 15、[websocket](https://developer.mozilla.org/zh-CN/docs/Web/API/WebSocket)

> 全双工的 通信api，用于实时聊天等场景，比起`http2.0`的长连接有着服务端主动发送消息的优势。

### 16、鬼一样的模块化

#### ES6Module（esm）

> 优势：
>
> ​	1、浏览器支持
>
> ​    2、ES6 模块**不是对象**，而是通过`export`命令显式指定输出的代码，再通过`import`命令输入，
>
> ​			被称为**【“编译时加载”或者静态加载】**是一种按需导入模式，比起`cjs`全部导出成对象要高效。
>
> 语法：
>
> ​	 不可以省略`.js`的文件类型
>
> ​	`export` 导出对象或者元素，`export default`默认导出挂载到导出对象的 `default`上
>
> ​	`import`导入 ，`*`引入所有，`as`重命名，`{default as xx}` 重命名默认

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

#### CommonJS(cjs)

>  `node.js` 支持

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

`exports`

>  隐式在头部声明 `var exports = module.exports`，即两者不能并存，存在覆盖关系

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

