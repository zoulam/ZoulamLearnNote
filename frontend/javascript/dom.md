# DOM

> ​ DOM是针对HMTL和XML文档的一个API。DOM描绘了一个层次节点树，允许开发人员添加、移除和修改页面的一部分。DOM脱胎于Netscape及微软公司创造的DHTML\(动态HTML\)，但现在它以及成为表现和操作页面标记的真正的跨平台、语言中立的方式。

## ②获取节点的方式

### 1）\(……By……\)

| function | 效果 |
| :--- | :--- |
| `document.getElementById()` |  |
| `document.getElementsByName()` |  |
| `document.getElementsByTagName()` |  |
| `document.getElementsByTagName()` 返回伪数组，使用`tag[index]`的方式使用 |  |
| `document.getElementsByClassName()` |  |

```markup
<!DOCTYPE html>
<html lang="zh">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <div class="box">我是一个：</div>
    <div class="box">我是一个：</div>
    <div class="box">我是一个：</div>
    <div class="box">我是一个：</div>
    <div class="box">我是一个：</div>
    <div id="div1">我是一个：</div>

    <form action="javascript;">
        <input type="text" name='text'>
    </form>
    <script>
        // 注：因为id是唯一的，所以只能获取第一个满足条件的节点

        // 获取 id="div1" 的节点
        var oDiv1 = document.getElementById('div1');
        // name这个属性其他HTML标签也可以设置但是只有表单元素form 中才生效
        // 获取 name='text' 的节点
        var oText = document.getElementsByName('text');


        // 下面两种方式获取的是类数组形式的对象值
        // 获取全部 tag div 的节点
        var oDiv2 = document.getElementsByTagName('div');
        // 获取第一个 tag div 的节点
        var oDiv2 = document.getElementsByTagName('div')[0];
        // 获取 class="box" 的节点
        var oBox = document.getElementsByClassName('box');

    </script>
</body>

</html>
```

#### getElementsByClassName不兼容解决方式

```javascript
//getElementsByTagName 实现兼容
function elementsByClassName(node, classStr) {
    //用兼容的方式获取node下的所有节点
    let nodes = node.getElementsByTagName("*");
    let arr = [];//存储符号条件的节点
    for (let i = 0; i < nodes.length; i++) {
        if (nodes[i].className === classStr) {
            //判断是否符合
            arr.push(nodes[i]);
        }
    }
    return arr;
}
```

### 2）\(querySelector\)

跟着**HTML5**一起出现的新型API，传入参数是CSS选择器的格式

```markup
    <div class="box">我是一个：</div>
    <div class="box">我是一个：</div>
    <div class="box">我是一个：</div>
    <div class="box">我是一个：</div>
    <div class="box">我是一个：</div>
    <div class="apple">im apple</div>
    <div id="div1">我是一个：</div>

    <form action="javascript;">
        <input type="text" name='text'>
    </form>
    <script>
        // H5实现：一种通用的实现方式,使用CSS选择的方式获取
        // 只选择第一个满足条件的节点
        var body = document.querySelector('div');
        var div1 = document.querySelector('#div1');

        // 选择全部class="box"的节点
        var Allbox = document.querySelectorAll('.box');
        var box = document.querySelector('.box');
        // body可以省略
        var body = document.body.querySelector('div.apple');
```

### ③CSS值修改

#### 获取有效样式

> ​ JavaScript中的CSS的value命名从**-**替换成驼峰命名 ,如：`background-color`等价于`backgroundColor`,可以通过赋值的方式修改。
>
> ​ 因为存在样式覆盖、优先级覆盖、样式继承等多种因素，所以手动计算有效样式是比较麻烦的事情，JavaScript给我们提供了这些API计算有效样式。

| 方式 | 环境 |
| :--- | :--- |
| `getComputedStyle(node)[cssStyle]` | 新型浏览器 |
| `node.currentStyle[cssStyle]` | IE |

```javascript
function getStyle(node, cssStyle) {
    return node.currentStyle ? node.currentStyle[cssStyle] : getComputedStyle(node)[cssStyle];
}
```

### 3）\(子节点\)

| 访问所有子节点\(缺点：将缩进内容也计入文本节点\) | 访问（除文本节点的外的子节点）缺点：IE8以下不兼容 |
| :--- | :--- |
| `childNodes`\(返回类数组\) | `children` |
| `fitstChild` | `firstElementChild` |
| `lastChild` | `lastElementChild` |
| `nextSibling` | `nextElementSibling` |
| `previousSibling` | `previousElmentSibling` |

#### 节点类型

`<div id="div1">I'm a div</div>`

​ ①元素节点 `<div></div>`

​ ②属性节点 `id = "div1"`

​ ③文本节点 `I'm a div`

#### 子节点的属性

|  | nodeType | nodeName | nodeValue |
| :--- | :--- | :--- | :--- |
| 元素节点 | 1 | 标签名 | null |
| 属性节点 | 2 | 属性名 | 属性值 |
| 文本节点 | 3 | \#text | 文本内容 |

#### 值操作

| 属性/方法 | 内容 |
| :--- | :--- |
| `value` | 表单元素的`value`和`input`输入的内容 |
| `innerHTML` | 该标签内的所有值，包含标签（但不包括本身的标签） |
| `outerHTML` | 标签的所有值 |
| `innerText` （禁止使用） | 标签内的纯文本值 |
| `outerText` （禁止使用） | 标签内的纯文本值 |
| `setAttribute()` | 设置或者修改行内属性如`id class value ……` 并返回`undefined` |
| `getAttribute()` | 获取行内的属性如`id class value ……` |

```markup
<!DOCTYPE html>
<html lang="zh">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
        window.onload = function () {
            var inP = document.getElementById("inP");
            var P = document.getElementById("P");
            var btn = document.getElementById("btn");
            btn.onclick = function () {
                //value
                console.log("inputvalue: " + inP.value);
                console.log("Pvalue: " + P.value);
                console.log("buttonvalue " + btn.value);
                console.log('-------------------------------------------');

                // 带标签
                console.log("innerHTML:" + P.innerHTML);
                console.log("outerHTML:" + P.outerHTML);
                console.log('-------------------------------------------');

                // 纯文本
                console.log("innerText: " + P.innerText);
                console.log("outerHTML: " + P.outerHTML);
                // function
                console.log('-------------------------------------------');

                // 获取p标签的name
                console.log("getAttribute: " + btn.getAttribute("value"));
                // 将P标签的name设置为123
                console.log("setAttribute: " + btn.setAttribute("name","123"));// 返回值为undefined
                console.log("getAttribute: " + btn.getAttribute("name"));
                // 将P标签的name从123改为456
                console.log("setAttribute: " + btn.setAttribute("name","456"));
                console.log("getAttribute: " + btn.getAttribute("name"));
            }
        }
    </script>
</head>

<body>
    <input type="text" id="inP">

    <div id="P">
        <p value="无效的value">5566555</p>
    </div>

    <button id="btn" value="btnValue">确定</button>
</body>

</html>
```

## 节点操作

| 操作方式 | 效果 | 场景 |
| :--- | :--- | :--- |
| `document.write("<h1>hello world</h1>");` | 覆盖当前页面的所有节点内容 | 出控制台和alert之外的输出方式 |
| `var oP = document.createElement("p");` | 创建并返回节点 | 适用于创建单独的节点 |
| `node1.appendChild(node2)` | 将node2插入到node1，成为他的子节点 | 对原有节点扩充 |
| `document.creatTextNode("<h1>hello world</h1>")；` | 创建文本节点：不会被HTML识别创建文本节点 |  |
| `var insertedNode = parentNode.insertBefore(newNode, referenceNode);` | 将newNode插入到referenceNode前面 |  |
| `parentNode.replaceChild(newChild, oldChild);` | 指定的节点替换当前节点的一个子节点（oldChild），并返回被替换掉的节点（newChild）。 |  |
| `let clone = node.cloneNode()` | 克隆节点，并返回 |  |
| 填入参数true`node.cloneNode(true)` | 深克隆，（克隆该节点和该节点的子节点） |  |
| `let oldChild = node.removeChild(child)` | 移除子节点，并返回被删除的节点 |  |

## 文档碎片

```javascript
    window.onload = function () {
        console.time("test1");
        for (let i = 0; i < 100000; i++) {
            var newDiv = document.createElement("div");
            // 一个一个往文档中添加
            document.body.appendChild(newDiv);
        }
        //每次的创建时间都有所不同，但是文档碎片的方式明显效率更高
        console.timeEnd("test1");//213ms

        //文档碎片：创建一个节点，后面的节点先插入那个节点，再插入文档
        console.time("test2");
        var node = document.createElement("div2")
        for (let i = 0; i < 100000; i++) {
            var newDiv = document.createElement("div");
            node.appendChild(newDiv)
        }

        // 最后才加入到文档中
        document.body.appendChild(node);
        console.timeEnd("test2");//100ms
    }
```

## ⑤事件（Event）

### 事件对象获取的封装

> 1、事件绑定后就产生事件对象
>
> 2、正常的函数执行的时候是需要执行符号的`();`
>
> 而事件绑定的函数是根据事件触发的，且事件对象会成为函数的第一个参数

```javascript
        function show(ev) {
            var ev = ev || window.event;//window.event 是兼容ie8以下的
            alert(ev);//[object MouseEvent]
            alert(arguments[0]);//[object MouseEvent]
        }
        window.onload = function () {
            var oBtn = document.getElementById("btn1");
            oBtn.onclick = show;//此处的show没有立即执行符号，但被事件触发后show方法被执行了，即被事件驱动
        }
```

> ​ 冒泡事件：子节点的效果向外传递**\[浏览器默认机制\]**
>
> ​ 事件捕获：父节点的效果向内传递

### 事件分类

#### 鼠标事件`[object Mouse]`

| 必须搭配on使用 |  |
| :--- | :--- |
| `click` |  |
| `dblclick` |  |
| `mouseover` | 鼠标移入（传递到子节点，子节点也生效） |
| `mouseout` |  |
| `mousemove` | 鼠标移动（不停触发） |
| `mouseenter` | 鼠标移入（不会传递到子节点） |
| `mouseleave` |  |
| `mousedown` | 点击未松开 |
| `mouseup` | 松开 |

**button事件对象的属性**

```javascript
        document.onmousedown = function (ev) {
            var ev = ev || window.event;
            alert(ev.button);
            // ev.button会根据点击鼠标的不同位置返回不同的值，这样就可以实现根据不同点击产生不同效果
            // 三个属性：
            // 0 按下鼠标左键
            // 1 按下滚轮
            // 2 按下鼠标右键

            // 位置属性：获取按钮点击时的位置信息
            // clientX clientY 原点位置：可视窗口的左上角
            // pageX pageY  原点位置：整个页面的左上角（滚动也会改变）
            // screenX screenY 原点位置：屏幕的左上角
            console.log(ev.clientX + " " + ev.clientY);
            console.log(ev.pageX + " " + ev.pageY);
            console.log(ev.screenX + " " + ev.screenY);
        }
```

> 位置信息可以使用在一些防止操作出界的场景

#### 鼠标+四个非输入按键（`onclick+other`）

```javascript
        window.onload = function () {
            document.onclick = function (ev) {
                var ev = ev || window.event;
                var arr = [];
                if (ev.shiftKey) {
                    arr.push("shift");
                    console.log("your enter shift and leftmouse");
                }
                if (ev.altKey) {
                    arr.push("alt");
                    console.log("your enter alt and leftmouse");
                }
                if (ev.ctrlKey) {
                    arr.push("ctrl");
                    console.log("your enter ctrl and leftmouse");
                }
                if (ev.metaKey) {
                    arr.push("windows");
                    console.log("your enter windows and leftmouse");
                }
                console.log(arr);
            }
        }
```

#### 键盘事件

| on | 效果 |
| :--- | :--- |
| `keydown` | 按住不放一直触发 |
| `keyup` | 键盘抬起 |
| `keypress` | 键盘操作（只支持字符键，读取输入内容） |

**键盘事件的属性\(which\)**

```javascript
        window.onload = function () {
            // window.onkeydown = function (ev) {
            //     var ev = ev || window.event;
            //     var which = ev.which || ev.keyCode;//keyCode（键码）只跟onkeydown匹配使用
            //     console.log(which);//返回键盘的大写值即不区分大小写，小写也是一样的值
            //     // ctrl 17
            //     // shift 16
            //     // window 91
            //     // alt 18
            // }
            window.onkeypress = function (ev) {
                // 支持输入内容的键，数字、字母
                var ev = ev || window.event;
                var which = ev.which || ev.charCode;//charCode（字符码）只跟onkeypress匹配使用
                console.log(which);//返回键盘的区分大小写的ASCII值，不支持功能键ctrl……
            }
        }
```

#### window事件

| 事件on+ |  |
| :--- | :--- |
| `load` 页面加载完成后触发 | 常用在保证JavaScript代码**保证**能读取到HTML文档内容 |
| `unload` 页面解构时触发（刷新页面、关闭页面）只在IE兼容 |  |
| `scroll`  页面滚动时触发 |  |
| `resize`页面尺寸变化时触发 |  |

#### 表单事件

|  |  |
| :--- | :--- |
| `blur`失去焦点 |  |
| `focus`获取焦点 |  |
| `select` 选中文本 |  |
| `change` 文本被修改，再失去焦点 |  |
| 必须绑定在input或者button上，本身就有功能，同时可以自己添加 |  |
| `sumbit` |  |
| `reset` |  |

#### 事件冒泡

**target属性**

> 子元素传递到父元素（内到外）

```markup
<!DOCTYPE html>
<html lang="zh">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
        /*
            target 目标对象/触发对象 IE8以下不兼容
            IE8的实现：window.event.srcElement
        */
        window.onload = function () {
            var oUl = document.getElementById("ul");
            oUl.onclick = function (ev) {
                alert(this.innerHTML);//this指向的是oUl
                var ev = ev || window.event;
                var target = ev.target || ev.srcElement;
                //点击li的时候事件冒泡传递到了ul，这就是事件冒泡存在的意义
                console.log(target.innerHTML);
            }
        }
    </script>
</head>

<body>
    <ul id="ul">
        <li id="li1">111</li>
        <li>222</li>
        <li>333</li>
    </ul>
</body>

</html>
```

**阻止事件冒泡**

> 没有阻止前 点击div2 **先打印 div2（子元素）后打印div1（父元素）**
>
> 这样点击内标签就不会触发父元素的事件

```javascript
<!-- 14事件冒泡深入.html -->
<!DOCTYPE html>
<html lang="zh">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div {
            padding: 50px;
        }

        #div1 {
            background-color: brown;
        }

        #div2 {
            background-color: blue;
        }

        #div3 {
            background-color: yellow;
        }
    </style>
    <script>
        window.onload = function () {
            var oDivs = document.getElementsByTagName("div");
            for (let i = 0; i < oDivs.length; i++) {
                oDivs[i].onclick = function (ev) {
                    ev = ev || window.event;
                    console.log(this.id);
                    //阻止冒泡的两种方式 都是归属于事件对象的属性或方法 存在兼容性问题，需要封装
                    // if (ev.stopPropagation) {//判断stopPropagation是否存在
                    //     ev.stopPropagation();
                    // }else{
                    //     ev.cancelBubble = true;
                    // }

                    //常用写法
                    ev.stopPropagation ? ev.stopPropagation() : ev.cancelBubble = true;
                }
            }
        }
    </script>
</head>

<body>
    <div id="div1">
        <div id="div2">
            <div id="div3"></div>
        </div>
    </div>
</body>

</html>
```

#### 事件委托

> 节省资源、能给**尚未存在**即后面用JavaScript生成的DOM节点同样获得事件

```markup
<!DOCTYPE html>
<html lang="zh">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
        window.onload = function () {
            var oUl = document.getElementById('ul1');
            var aLis = oUl.getElementsByTagName('li');
            // for (let i = 0; i < aLis.length; i++) {
            //     aLis[i].onclick = function () {
            //         aLis[i].style.backgroundColor = 'red';
            //     }
            // }
            //缺点:1给5个li标签都添加了相同的点击事件，浪费资源
            //     2后续添加的节点无法获取点击事件

            //事件委托就是为了解决这些问题
            //实现方式，给父节点（或祖先节点）添加事件
            //找到触发对象


            // ul委托li标签 执行变红
            oUl.onclick = function (ev) {
                var ev = ev || window.ev;
                var target = ev.target || ev.srcElement;
                if (target.nodeName.toLowerCase() == 'li') {
                    target.style.backgroundColor = 'red';
                }

            }

            var oBtn = document.getElementById('btn1');
            var i = 6;
            oBtn.onclick = function () {
                var newNode = document.createElement('li');
                newNode.innerHTML = i++ * 11111;
                oUl.appendChild(newNode);
            }
        }
    </script>
</head>

<body>
    <button id="btn1">新增节点</button>
    <ul id="ul1">
        <li>12345</li>
        <li>12345</li>
        <li>12345</li>
        <li>12345</li>
        <li>12345</li>
    </ul>
</body>

</html>
```

#### 事件监听

> `EventListener()`,存在兼容性问题

```markup
<!DOCTYPE html>
<html lang="zh">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
        /*
            传统事件的缺陷
            1.重复添加时会被覆盖
            2.无法精确删除事件函数，只能通过赋值覆盖

            事件监听器
            使用场景：
            1.一个事件绑定多个函数
            2.需要精确删除某个事件函数

            addEventListener()
            使用方式：
            node.addEventListener()
            参数
            arg1 事件类型
            arg2 绑定事件函数
            arg3 boolean true 事件捕获
                         false（default）事件冒泡

            removeEventListener()
            使用方式：
            node.removeEventListener()
            参数
            arg1 事件类型
            arg2 删除事件函数名

         */
        window.onload = function () {
            var oBtn = document.getElementById('btn1');

            // oBtn.onclick = function () {
            //     alert('点击');
            // }
            // oBtn.onclick = function () {
            //     alert('点击2');
            // }
            oBtn.addEventListener('click', () => {
                console.log('点击1');
            }, false)
            oBtn.addEventListener('click', () => {
                console.log('点击2');
            }, false)

            var aBtns = document.getElementsByTagName('button');
            // aBtns[1].onclick=function(){
            //     console.log("本身的点击函数");
            // }
            // aBtns[0].onclick=function(){
            //     aBtns[1].onclick=show;
            // }
            // aBtns[2].onclick=function(){
            //     aBtns[1].onclick=null;//将本身的点击函数也删除了
            // }
            aBtns[1].addEventListener('click', () => {
                console.log("本身的点击函数");
            }, false)
            aBtns[0].onclick = function () {
                aBtns[1].addEventListener('click', show, false);
            }
            aBtns[2].onclick = function () {
                aBtns[1].removeEventListener('click', show)
            }
        }

        function show() {
            console.log('show the world');
        }
    </script>
</head>

<body>
    <button>添加函数</button>
    <button>按钮</button>
    <button>删除函数</button>
    <button id="btn1">事件监听</button>
</body>

</html>
```

**事件监听实现兼容**

```javascript
//浏览器兼容ie 事件监听函数：attachEvent() detachEvent()
        function addEvent(node, eventType, funcName) {
            node.addEventListener ? node.addEventListener(eventType, funcName, false) : node.attachEvent("on" + eventType, funcName);
        }
        function removeEvent(node, eventType, funcName) {
            node.removeEventListener ? node.removeEventListener(eventType, funcName) : node.detachEvent("on" + eventType, funcName);
        }
```

#### 事件捕获

> 从子元素传递到父元素，由内到外
>
> **点击div2 先打印div1（父元素） 后打印div2（子元素）**

```markup
<!-- 16事件捕获.html -->
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div {
            padding: 50px;
        }

        #div1 {
            background-color: red;
        }

        #div2 {
            background-color: orange;
        }

        #div3 {
            background-color: blue;
        }
    </style>
    <script>
        window.id
        window.onload = function () {
            var aDivs = document.getElementsByTagName('div');
            for (let i = 0; i < aDivs.length; i++) {
                aDivs[i].addEventListener('click', () => {
                    //箭头函数的this是指向 不是aDivs[i]
                    // console.log(this.id);
                    console.log(aDivs[i].id);
                }, true);
            }
        }
    </script>
</head>

<body>
    <div id="div1">
        <div id="div2">
            <div id="div3"></div>
        </div>
    </div>
</body>

</html>
```

## 练习和DEMO

