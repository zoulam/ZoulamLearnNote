# \[js\]DOM02

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

> 冒泡事件：子节点的效果向外传递**\[浏览器默认机制\]**
>
> 事件捕获：父节点的效果向内传递

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

```javascript
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

```javascript
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

#### 事件监听（EventListener）

> `EventListener()`,存在兼容性问题

```javascript
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

```javascript
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

## 位置信息

| 操作方式 | 效果 |
| :--- | :--- |
| `鼠标事件.clientX cilentY` | 鼠标事件距**可视窗口左上角**的距离 |
| `鼠标事件.pageX pageY` | 鼠标事件距**页面左上角**的距离，即页面发生滚动之后原点不变 |
| `鼠标事件.screen.X screenY` | 鼠标事件距**屏幕左上角**的距离，当页面不是全屏的时候发生变化 |
| `node.offsetLeft offsetTop offsetWidth offsetHight` | 获取（节点）距**父节点**的距离 |
| `UI事件.layerX layerY` 【处于测试阶段】 | UI事件当前层中的水平坐标 |

