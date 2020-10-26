# 重要知识速记

# HTML

```javascript
!DOCTYPE html
html lang
head
    meta
        charset
        name:keywords、description、viewport content
        http-equiv content
    title
link （href）
script defer="defer" async type="text/script" "module" src
style rel="stylesheet" href
body

src :script source img input(image) iframe
href:link a

hgroup header footer nav aside section article
 strong del select progress
source > audio source > video

ol>li
ul>li
dl>li

自闭 input img link meta br hr area ifame source

from(块) action entype method
input(行、自闭)  name value autocomplete=“off” readonly autofocus
                type:file text color checkbox(多选、依靠name分组) radio（单选）
                    button submit reset date random search email
          <button></button> 非自闭

 table: thead tbody tfoot
                 tr
                     td(colspan合并行、rowspan合并列)
```

# CSS

```JavaScript
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

可继承的属性
		




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

# JS

## 1、types

```javascript
声明方式
var(变量提升、全局作用域) let(块级作用域) const（必须初始化，基本数据类型不可修改，引用数据类型的元素可修改）

基本数据类型（必须小写、不然会与包装类重名）
number（包含以下内容）：
(float int Infinity NaN 【-2 ** 53 ~ 2 ** 53 -1】【1e6 == 1000000】 1e9 + 7) 
string undefined null bigInt symbol boolean
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

判断方式
typeof ---- 小写（function null(object) 基本数据类型
Object.prototype.toString.call()---- [object 大写] 精准
instanceof ---- 返回boolean，任何数据instanceof Object都是true
constructor ---- 获取构造器

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
~ 按位非 ~a -(a+1) ~15 == -16
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

## 2、proto

prototype是函数特有结构，`__proto__`是对象属性是原型链的链条

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

## 3异步编程

### ①回调函数

### ②generator

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

### ③promise

```
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



### ④async、await

## 4、regexp

```JavaScript
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
[] 范围
[^0-9] 非数字
* 0到n
+ 1到n
? 0或1 还有贪婪的意思
{n,m} [n, m] {n,} [n, +∞]
^ 以什么开头
$ 以什么结尾

(?=) 后缀
(?<=) 前缀
(?!) 不能带这个后缀
(?<!) 不能带这个前缀
```



## 5、闭包&立即执行函数

```JavaScript
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

## 6、ajax

**原生xhr对象使用繁琐**

```
1、实例化对象
2、open('GET','URL')
3、send()
4、监听
	xhr.onReadyStateChange = function(){
		xhr.readyState 
			0	open() 前
			1	open() 后 send()前
			2	send() 后 响应前
			3	响应传输完成前
			4	响应全部传输完成
		xhr.status [200-299] 或 [304]
		xhr.response
	}
```

### fetch

1、认为400、500响应码是正常的 

~~2、不支持abort~~

3、信息不够丰富（progress）

4、默认不带cookie

```JavaScript
fetch('url').then((response) => {opt})
```



### axios

```
1、支持node（httpproxy）、web
2、返回promise
3、最基本使用的方式:axios.get('url', ).then((response)=>{opt})
```

## 7、跨域

```javascript
跨域是指（协议、域名、端口）有任何一个不同，浏览器出于安全考虑禁止http传输
iframe、script、form等历史遗留的标签不存在这个问题
jsonp：通过url发送get请求，通过query字符串确定变量名
cors：跨域资源共享服务端设置Access（通道）
		res.setHeader("Control-Allow-Origin", url)
		res.setHeader("Control-Allow-Origin", *) // 无法携带cookie，可设置白名单对象
开发模式：1、http proxy 2、nginx
```

### 1、jsonp

**客户端**

```JavaScript
    <div id="jsonp"></div>
    <script>
        function handle(data) {
            document.getElementById('jsonp').innerHTML = data.test;
        }
    </script>
    <script src="http://localhost:3000/jsonp-server?Func=handle"></script>
```

**服务端**

```JavaScript
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

### 2、cors

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

## 8、this

```javascript
0、改变this指向的方式 call apply bind new
1、箭头函数没有this、他能获取到的(别人的)this是【不可修改的，在声明时就确定的】
2、
	2.1未实例化：
	非 `use strict` 时执行全局（window || global）， `use strict`就是undefined
	2.2实例化：
	指向实例化之后生成的对象
```



### 谁用就是谁

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

### 离得近

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

### 改变this的优先级

**结论：new > call** 

```JavaScript
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

### 箭头函数尝试改变this

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

```JavaScript
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

## 9、function

```
默认返回值是undefined
其他时候返回this
```

## 10、技巧

### compose

**组合函数按从右往左顺序执行，右边的返回值是左边的参数**

```javascript
function compose(...funcs) {
    if (!funcs.length) return arg => arg;
    if (funcs.length === 1) return funcs[0];
    return funcs.reduce((a, b) => (...args) => a(b(...args)))
}
```

### 柯里化

```

```

### 解构

```javascript
1、更新对象
oldObject，newItem

oldObject = {...oldObject, [id]: newItem}

function MyComponent({state, props, ...rest}){}
```

### 偷方法

```javascript
let obj = {length:0}
Array.prototype.[func].call(obj, ...args)
// obj从数组的原型上获取了func方法
```

### 二维数组

```javascript
// 创建5 * 5的二维数组
let doubleArr = Array.from({ length: 5 }, () => new Array(5))
```



## 11、花式继承

## 12、EventLoop

