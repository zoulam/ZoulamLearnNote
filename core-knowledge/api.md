---
description: Bom Dom Array String Set Map
---

## 两类API

1、`<ObjectName>.prototype.Func()` ,通过原型链调用，或指定`实例化对象.方法`也能调用

2、`<ObjectName>.Func()` 只要是实例化自指定对象的都能调用

## 类数组

> 定义类数组，给对象添加`length`属性,下面列举三种给类数组添加方法的方式

```javascript
// 类数组的关键是给对象添加lenth属性
let obj = {
    0: 'zoulam',
    1: 'luluxi',
    2: 'lala',
    3: 'momo',
    'length': 4,
    // 非侵式添加
    'slice': Array.prototype.slice
}


let nums1 = obj.slice(0, 2)
console.log(nums1);// [ 'zoulam', 'luluxi' ]
// 入侵式添加【所有的对象都会添加上这种方法】
Object.prototype.splice = Array.prototype.splice;
obj.splice(0, 1, 'dongdong');
console.log(obj);
// {
//     '0': 'dongdong',
//     '1': 'luluxi',
//     '2': 'lala',
//     '3': 'momo',
//     length: 4,
//     slice: [Function: slice]
//   }

// 委托方式添加
let push = Array.prototype.push
push.call(obj, 'didi');
console.log(obj);
// {
//     '0': 'dongdong',
//     '1': 'luluxi',
//     '2': 'lala',
//     '3': 'momo',
//     '4': 'didi',
//     length: 5,
//     slice: [Function: slice]
//   }
```

## 字符串

`split(str/regex)` return Array

`charCodeAt(index)` 返回ASCII

 `charAt(index)` 等同于 `s[index]`

`slice(startIndex, endIndex)` 裁切 左闭右开 [startIndex, endIndex)

`subString(startIndex, endIndex)` 效果同上

`trim()` `trimRight()` `trimLeft()`

`replace(oldString/ regexp, newString/callback)`

​		callback(match, lastIndex, oldStr)

​		原理是迭代遍历，遍历的起点是lastIndex

`match()` return array / boolean

`toLowerCase()`

 `toUpperCase()`

`s.startsWith(str)` 以什么开头，返回boolean

`includes(subStr)` 是否包含,返回boolean

`indexOf(char)` 返回首次出现的下标，**没有返回-1**

## 数学

常数 `Math.PI`

`Math.max(...args)`

`Math.min(...agrs)`

`Math.abs(number)`

`Math.sqrt(number)` 

`Math.pow(a, b)` 指数运算，等价于  a ** b

`Math.floor(number)`   理解为下楼梯

​	输入 3.9返回3 

​	输入-3.1 返回-4

`Math.trunc【截断】(number)`  抹零

​	输入3.1 / 3.9 输出 3

​	输入-3.1/-3.9 输出3

`Math.round(number)` 四舍五入

​	输入 -3.6 输出 -4

## 数字 [more](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number)

`Number()` 不断尾，返回NaN

`parseInt(string, radix)` 断尾，不断头

`number.parseInt()`

`parseFload(string)`

`isNaN(val)` 比起 `(NaN == NaN)` false 好用

`isFinite()` 数字除以0或者 `Infinity`

全局变量 `Infinity` 无穷

`numberObj.toFixed()` 保留多少位小数

`numberObj.toPrecision(位数)` 【Precision：中文意思精度】

## 数组

### 原地操作

**涉及到length长度的变化的情况，不要忘记缓存length**

`push(...agrs)`  后入

`unshift(...args)` 前入

`pop()`  后出

`shift()` 前出

`splice(startIndex, length, ...spliceContent)` 

​	当length为0时在`startIndex`后面插入内容

​	当length为1的时候替换掉`startIndex`位置的内容

​	当length大于1，从`startIndex`位置开始替换

`reserve()` `sort((a, b)=> a - b)`

`forEach((element, index, array)=>{ element doSomething})` oldArray

### 新数组上操作

`concat()` 返回新数组 `let d = a.concat(b,c)`  **等价于** `let d = [...a, ...b, ...c]`

`filter((element, index, array)=>{}, this)`  过滤

​	回调函数返回值为true 就push到newArr，newArr是新的返回值

​	**第二个参数是可以使用this获取的**

`find((element, index, array)=>{}, this)` 

等效于filter的短路操作，返回的是第一个回调函数返回true数组元素

`map((element, index, array)=>{element doSomething},this)` 

​	与forEach（**遍历**）一致newArr

`reduce((first, current, index, Array)=>{},first)`

​	first默认是数组的第一项，第二个参数传入时可作为初始值

`join(',')` === `toString()` return String

`slice(startIndex, endIndex)` 左闭右开 `[startIndex, endIndex]`

## 日期

`Data().now`

## Map

`let map = new Map()`

`size`

**参数为不填或者填入key**

`has(key)`

`set([key,value])`

`get(key)`

`delete(key)`

`clear()`

`keys()` 可迭代的`key`

```javascript
for (let key of map){

}
```

`values()` 可迭代的`value`

```javascript
for (let [key, value] of map){

}
```

`entries()` 可迭代的`[['key', 'value']]`

```javascript
var myMap = new Map();
myMap.set("0", "foo");
myMap.set(1, "bar");
myMap.set({}, "baz");

var mapIter = myMap.entries();

console.log(mapIter.next().value); // ["0", "foo"]
console.log(mapIter.next().value); // [1, "bar"]
console.log(mapIter.next().value); // [Object, "baz"]
```

`forEach()`

`size`

## Set

**传入的参数都是key、或不穿**

`let set = new Set([1, 2, 3, '3'])`

`add()`

`clear()`

`delete()`

`has()`

`forEach()`

`values()`

`entries()` 返回二维数组 `[[key1, value1], [key2, value2]]`

内含迭代器，可用`for of`,不考虑顺序就 `for in`

`size` 属性，去重之后的长度

```javascript
var mySet = new Set();
mySet.add("foobar");
mySet.add(1);
mySet.add("baz");

var setIter = mySet.entries();

console.log(setIter.next().value); // ["foobar", "foobar"]
console.log(setIter.next().value); // [1, 1]
console.log(setIter.next().value); // ["baz", "baz"]
```

> 数组去重

```javascript
let oldArray = [1, 2, 3, '3', 3, 3]
let newArray = [... new Set(oldArray)]
```

## Object

### 对象方法

​	`Object.assign(obj1, obj2)` 对象1的同名 `key`的 `value`会被对象2覆盖，用于合并配置

​	`create({})` 拷贝对象的 到函数的 `prototype`

### 对象原型方法

`Object.prototype.hasOwnProperty()`  查看对象的属性和方法是否挂载在对象上而不是原型上

​	使用方式：`obj.hasOwnProperty('key')`

## WeakMap

map是用两个互相映射的内容，分别存储 `[key, val]`，互相引用会出现无法清除的情况，**导致内存泄漏**，`WeakMap`就是创建这种弱引用的。

`delete()`

`get()`

`has()`

`set()`

属性`length`

## WeakSet(不可枚举)

**只能存储对象的集合**

`add()`

`delete()`

`has()`

`length`

## dom

### 标签选择器

​	获取单个、获取`nodeList`即伪数组

### DOM节点的增删改

| 操作  （A为父节点bc为兄弟子节点，D为document的简写） | 效果                                                   |
| ---------------------------------------------------- | ------------------------------------------------------ |
| `D.write()`                                          |                                                        |
| `D.createElement()`  `D.createDocumentFragment`      | 前者再原文档创建，后者在空白文档创建                   |
| `A.appendChild()`                                    |                                                        |
| `D.createTextNode()`                                 |                                                        |
| `A.replaceChild(b, c)`                               | **b换c**                                               |
| `A.insertBefore(b, c)`                               | **b插在c前面**                                         |
| `node.clone()`                                       | 含参数，默认是是`false` ，`true`是深拷贝，即包含子节点 |
| `removeChild()`                                      |                                                        |

### 行内属性的获取和设置，节点包含的（常用）属性

通常会使用 `attributes` 获取全部属性(**包含自定义属性**，以类数组的形式)，然后再具体操作

```JavaScript
      // 拆分键值对的方式，解构出来的只有自定义属性和类名
	let attrs = oName.attributes;
        [...attrs].forEach(attr => {
            // 解构获取键值 name 和 value 如有必要可以进行重命名
            let { name, value : expr } = attr;
            console.log(name, value);
        })
```

`innerText（不包含子元素的文本） textContent（包含子元素的文本） innerHTML outerHTML value(表单元素特有) classList（类名列表）`

增删改

`element.getAttribute(name)`

`element.setAttribute(name, value)`

`element.removeAttribute(keyName)`

### 父子、兄弟节点

三个判定属性 `nodeType` `nodeName` `nodeValue`

[more](https://developer.mozilla.org/zh-CN/docs/Web/API/Node)

#### 父亲节点

`parentNode`

#### 子节点列表

以下两个会出无法跳过换行，即会读取文本节点：`childNodes`  `firstChild` `lastChild`

只读取元素节点：  `children`  `firstElementChild`  `lastElementChild`

#### 兄弟节点

`nextSibling` `previousSibling`

### 自定义数据

`data-*`

`node.dataset`

复杂数据

`node.getAttribute("data-*")`

`JSON.parse(node.getAttribute('data-*'))` 解析成 `JSON`对象

### 节点位置信息

滚动到底时满足等式： `scrollHeight - scrollTop = clientHeight`

|      | 偏移（基本等于你设置）                        | 滚动                                                | 客户端                                        |
| ---- | --------------------------------------------- | --------------------------------------------------- | --------------------------------------------- |
| 描述 | **（content+padding+border）**或**（width）** | （无溢出时与client相等）（溢出时：client+溢出大小） | （**content+padding**）或（**width-border**） |
|      | offsetWidth                                   | scrollWidth                                         | clientWidth**（不含border、margin、滚动条）** |
|      | offsetHeight                                  | scrollHeight                                        | clientHeight                                  |
|      | offsetLeft **（外边距距离父元素的距离）**     | scrollLeft**（可写、滚动条位置）**                  | clientLeft**（边框非transport才有效）**       |
|      | offsetTop                                     | scrollTop                                           | clientTop                                     |

![示例](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/201211191754284481.png)

#### 鼠标事件位置

| offset                                       | client                           | page                           | screen（）             |
| -------------------------------------------- | -------------------------------- | ------------------------------ | ---------------------- |
| 实验性api（鼠标点击位置与元素padding的距离） | （滚动和缩放都不会改变原点位置） | （页面无滚动条时与client相同） | （全屏时与client相同） |
| offsetX offsetY                              |                                  |                                |                        |

[看这篇文章把](https://juejin.im/post/6844903854157332487)



### 常用事件

**MouseEvent** [more](https://developer.mozilla.org/zh-CN/docs/Web/API/MouseEvent/MouseEvent)

meta:中文意思是可变化的意思

`click` 

​	`MouseEvent.shiftKey` 

​	`MouseEvent.ctrlKey` 

​	`MouseEvent.altKey` 

​	`MouseEvent.metaKey【win或mac】`

`dbclick`

`mouseover` `mouseout`  传递到子元素

`mouseenter` `mouseleave`  不传递到资源

`mousemove`

`mousedown` 按下未松开

`mouseup` 松开

**KeyboardEvent**[more](https://developer.mozilla.org/zh-CN/docs/Web/API/KeyboardEvent/KeyboardEvent)

`keyup`

`keydown`  `event.keyCode` (读取不区分大小写的字母和 `shift`等快捷键)

`keypress`  `event.charCode` （读取数组和区分大小写的字母）

**HTMLFormEvent** [more](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLFormElement)

`blur` `focus` `select` `change`

这两个跟按钮的效果是一样的：`submit` `reset`

### 事件冒泡和捕获

冒泡：点击**子节点**，从子节点向父节点传递，直至 html文档。 **p -> div -> body -> html -> document**

捕获：点击**子节点**，从父节点向子节点传递，直至当前点击节点。

### 阻止事件

`event.preventDefault()` 阻止默认事件（如：点击超链接会跳转超链接），但不会阻塞后续事件

`event.stopPropagation【传播】()` 阻止事件冒泡或者捕获的传播，**而不能阻止当前事件**

`event.stopImmediate【立即】Propagation()` 阻止后续相同类型的事件执行

### 事件委托【设计模式中的代理模式】

> 通过**父节点或祖先节点**给不存在的**子节点**绑定相应的事件,关键是使用 `event.target.nodeName` 获取创建的子节点

```javascript
<!-- 14事件委托.html -->
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



### 文档碎片

> 大量插入节点时提高效率的方式，
>
> 1、先创建一个父节点，
>
> 2、再将文档碎片插入到父节点中，
>
> 3、最后再将父节点插入到指定的位置

### 类名数组

[Element.classList](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/classList)

包含丰富的类名处理功能

## BOM

**主要了解两个与路由有关的api**

### screen

### window

### history

### location

