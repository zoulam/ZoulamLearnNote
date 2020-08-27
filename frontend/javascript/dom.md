# DOM

>  	DOM是针对HMTL和XML文档的一个API。DOM描绘了一个层次节点树，允许开发人员添加、移除和修改页面的一部分。DOM脱胎于Netscape及微软公司创造的DHTML\(动态HTML\)，但现在它以及成为表现和操作页面标记的真正的跨平台、语言中立的方式。

## ②获取节点的方式

### 1）\(……By……\)

| function | 效果 |
| :--- | :--- |
| `document.getElementById()` |  |
| `document.getElementsByName()` |  |
| `document.getElementsByTagName()` |  |
| `document.getElementsByTagName()` 返回伪数组，使用`tag[index]`的方式使用 |  |
| `document.getElementsByClassName()` |  |

```JavaScript
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

```javascript
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

>  JavaScript中的CSS的value命名从**-**替换成驼峰命名 ,如：`background-color`等价于`backgroundColor`,可以通过赋值的方式修改。
>
>  因为存在样式覆盖、优先级覆盖、样式继承等多种因素，所以手动计算有效样式是比较麻烦的事情，JavaScript给我们提供了这些API计算有效样式。

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

 ①元素节点 `<div></div>`

 ②属性节点 `id = "div1"`

 ③文本节点 `I'm a div`

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
| `setAttribute()` | 设置或者修改行内属性如`id class value ……` 并返回`undefined`，即无返回值 |
| `getAttribute()` | 获取行内的属性如`id class value ……` |

```JavaScript
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
                console.log("setAttribute: " + btn.setAttribute("name","123"));// undefined
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

