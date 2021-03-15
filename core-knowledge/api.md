---
description: Bom Dom Array String Set Map
---

# API

## 两类API

1、`<ObjectName>.prototype.Func()`

在对象的 `__proto__`上，即每个实例化对象上面都挂在着

2、`<ObjectName>.Func()`

在构造函数上**【无法查看】**

`Array.form()`

## 1、类数组

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

## 2、字符串【都是返回新值】

`split(str/regex)` return Array

`charCodeAt(index)` 返回ASCII

`charAt(index)` 等同于 `s[index]`

`String.fromCharCode(number)`

```javascript
String.fromCharCode(97)// 'a'
String.fromCharCode(122)// 'z'
String.fromCharCode(65)// 'A'
String.fromCharCode(90)// 'Z'
```

`slice(startIndex, endIndex)` 裁切 左闭右开 \[startIndex, endIndex\)

`subString(startIndex, endIndex)` 效果同上

`trim()` `trimRight()` `trimLeft()`

`replace(oldString/ regexp, newString/callback)`

callback\(match, lastIndex, oldStr\)

原理是迭代遍历，遍历的起点是lastIndex

`match()`

return array

非全局的正则就返回一个跟exec一样的数组，

全局下的就返回一个配匹配到的数组

`toLowerCase()`

`toUpperCase()`

`s.startsWith(str)` 以什么开头，返回boolean

`includes(subStr)` 是否包含,返回boolean

`indexOf(char)` 返回首次出现的下标，**没有返回-1**

## 3、数学

常数 `Math.PI`

`Math.max(...args)`

`Math.min(...agrs)`

`Math.abs(number)`

`Math.sqrt(number)`

`Math.pow(a, b)` 指数运算，等价于 a \*\* b

`Math.floor(number)` 理解为下楼梯

输入 3.9返回3

输入-3.1 返回-4

`Math.trunc【截断】(number)` 抹零

输入3.1 / 3.9 输出 3

输入-3.1/-3.9 输出3

`Math.round(number)` 四舍五入

输入 -3.6 输出 -4

## **∞需要先说明的迭代器**

**以下图片皆来自高程4pdf**

<img src="https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201218233135254.png" alt="iterator" style="zoom:67%;" />



<img src="https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201218233300848.png" alt="isHas iterator" style="zoom:50%;" />

```javascript
console.log(arr[Symbol.iterator]() == arr.values());// false
console.log(arr[Symbol.iterator] == arr.values);// true
```

### 自行声明迭代器

> 1、 迭代器的返回值是一个对象，里面包含next函数
>
> 2、next函数的返回值是对象，包含`value`和`done`两个信息

```javascript
let obj = {
    value: ['zoualm', 'lala', 'momo'],
    [Symbol.iterator]() {
        let index = 0
        let that = this
        return {
            next() {
                // 此处可以简化为三元表达式
                if (index == that.value.length) {
                    return { done: true, value: undefined }
                } else {
                    return { done: false, value: that.value[index++] }
                }
            }
        }
    }
}

for (let value of obj) {
    console.log(value)
}
```



## 4、数字 [more](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number)

`Number()` 不断尾，返回NaN

`parseInt(string, radix)` 断尾，不断头

返回NaN的情况：

1、`string`不能转化为数字

2、`radix`不在2-36之间

```javascript
['1', '7', '11'].map(parseInt)//[1, NaN, 3]
// parseInt('1', 0) 1
// parseInt('7', 1) NaN
// parseInt('11', 3) 3
["1", "2", "3"].map(parseInt)// [1, NaN, NaN] 
// parseInt('1', 0) 10进制 1
// parseInt('2', 1) 越界
// parseInt('3', 2) 越界
// 解释如下
    parseInt()被map传入三个数据分别是 (item, index, array)

var parseInt = function(string, radix, array) {
    return string + "-" + radix + "-" + array;
};

["1", "2", "3"].map(parseInt);//  ["1-0-1,2,3", "2-1-1,2,3", "3-2-1,2,3"]
```

`number.parseInt()`

`parseFload(string)`

`isNaN(val)` 比起 `(NaN == NaN)` false 好用

`isFinite()` 数字除以0或者 `Infinity`

全局变量 `Infinity` 无穷

`numberObj.toFixed()` 保留多少位小数

`numberObj.toPrecision(位数)` 【Precision：中文意思精度】

```javascript
let num = 12; num.toPrecision(3); // 返回 字符串类型的 "12.0"
```

## 5、数组

## **⑤回调函数、高阶函数**

>  回调函数是作为参数传入到另一个函数中，他有两个特征，**自动获取参数，自动执行**，这两个特征是高阶函数赋予他的能力。

```javascript
new Promise((resolve, reject) => {
    // resolve('resolve something')
    reject('reject something')
}).then(console.log, console.error)

new Promise((resolve, reject) => {
    // resolve('resolve something')
    reject('reject something')
}).then((value) => {
    console.log(value);
}, (reason) => {
    console.error(reason)
})
```

> 高阶函数是指**能够接受回调函数作为参数的函数**，数组中的`map`，`filter`……都被称为高阶函数，日常使用中的**防抖节流**函数都是高阶函数

```javascript
// es5
function highOrderFunction(func) {
    var context = this
    return function () {
        func.apply(context, arguments)
    }
}

// es6
function highOrderFunction(func) {
    return (...args) => {
        func(...args)
    }
}
```

### 原地操作

**涉及到length长度的变化的情况，不要忘记缓存length**

`push(...args)` 后入

`unshift(...args)` 前入

`pop()` 后出

`shift()` 前出

`sort((a, b) => a - b)` a小 b大

将内容转化为字符串再进行**UTF-16**码的比较

再根据回调函数的返回值判断行为

a - b &lt; 0 \|\| a - b == 0 不交换

a - b &gt; 0 交换

`splice(startIndex, length, ...spliceContent)`

当length为0时在`startIndex`后面插入内容

当length为1的时候替换掉`startIndex`位置的内容

当length大于1，从`startIndex`位置开始替换

`reserve()`

`forEach((element, index, array)=>{ element doSomething})` oldArray

### 新数组上操作

> 第二个参数的`this`指回调函数的`this`

```javascript
Array.prototype.myFilter = function () {
    cb.apply(arguments[1] | window | global)
}
```

`concat()` 返回新数组 `let d = a.concat(b,c)` **等价于** `let d = [...a, ...b, ...c]`

`filter((element, index, array)=>{}, this)` 过滤

回调函数返回值为true 就push到newArr，newArr是新的返回值

```javascript
console.log([1, 2, 3, 4, 5].filter((item, index, curArray) => item % 2))
// [ 1, 3, 5 ]
// 偶数 0 false 舍去
// 奇数 1 true push
```

**第二个参数是可以使用this获取的**

`find((element, index, array)=>{}, this)`

等效于filter的短路操作，返回的是第一个回调函数返回true数组元素

`map((element, index, array)=>{element doSomething},this)`

与forEach（**遍历**）一致newArr

`reduce((first, current, index, Array)=>{},first)`

first默认是数组的第一项，第二个参数传入时可作为初始值

```javascript
// 以index0为初始值，第一次进入循环则是index 1
// 第一次进入循环：1 2 1 [ 1, 2, 3, 4, 5 ]
[1, 2, 3, 4, 5].reduce((accumulator, currentValue, index, originArray) => {
    console.log(accumulator, currentValue, index, originArray);
    return accumulator + currentValue
})


// 注入7为初始值，第一次进入循环则是index 0
// 第一次进入循环：7 1 0 [ 1, 2, 3, 4, 5 ]
[1, 2, 3, 4, 5].reduce((accumulator, currentValue, index, originArray) => {
    console.log(accumulator, currentValue, index, originArray);
    return accumulator + currentValue
}, 7)
```

`join(',')` === `toString()` return String

`slice(startIndex, endIndex)` 左闭右开 `[startIndex, endIndex]`

`every()` 返回`boolean`只有数组的全部元素都返回true的时候才是true

```javascript
let example = [3, 4, 5]
example.every((item)=> item >= 3) // => true
```



## 6、日期

`Data().now`

## 7、Map

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

返回二维数组 `[[key1, value1], [key2, value2]]`**这里联想一下结构赋值就好理解了，不要反应不过来**

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

## 8、Set

**传入的参数都是key、或不传**

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

## 9、WeakMap

`map`是用两个互相映射的内容，分别存储 `[key, val]`，互相引用会出现无法清除的情况，**导致内存泄漏**，`WeakMap`就是创建这种弱引用的。

`delete()`

`get()`

`has()`

`set()`

属性`length`

## 10、WeakSet\(不可枚举\)

**只能存储对象的集合**

`add()`

`delete()`

`has()`

`length`

## 11、Object

### 对象构造函数方法

`Object.defineProperty(obj, key, {description})`

> `interceptor`骚操作，只要获取值、设置值就会执行指定的 `getter() setter()`函数

```javascript
function DataArr() {
    var _val = null,
        _arr = [];
    Object.defineProperty(this, 'val', {
        get: function () {
            console.log('run getter');
            return _val;
        },
        set: function (newVal) {
            _val = newVal;
            _arr.push({ val: _val });
            console.log("set new value: ", _val);
        }
    });

    this.getArr = function () {
        return _arr;
    }

}

var dataArr = new DataArr();

dataArr.val = 123;
dataArr.val = '234';
console.log(dataArr.getArr());
console.log('---------------------------------------------------------------------------');
dataArr.val; // 获取值
```

![image-20201125002553189](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201125002553189.png)

`Object.assign(obj1, obj2)` 对象1的同名 `key`的 `value`会被对象2覆盖，用于合并配置

```javascript
        let obj1 = {
            dbName: "zoulam",
            connectionCollection: "lala"
        }
        let obj2 = {
            tableName: "runTime",
            connectionCollection: "lulu"
        }
        const obj3 = Object.assign(obj1, obj2);
        console.log(obj3);
        // connectionCollection: "lulu"
        // dbName: "zoulam"
        // tableName: "runTime"
```

`Object.create({})` 拷贝对象的 到函数的 `prototype`

```javascript
// 手写create
Object.myCreate = function(p) {
    function f(){};
    f.prototype = p;
    return new f();
}
```

`Object.keys()` 返回可迭代的键名数组

`Object.valus()` 返回可迭代的值数组

`Object.entries()` 返回可迭代的二维**键值**数组

```text
        const people = {
            name: "zoulam",
            age: 18
        }
        // 这里用了结构赋值，以前不太理解
        for (const [key, value] of Object.entries(people)) {
            console.log(key, value);
        }
        // name zoulam
        // age 18
```

`Object.setPrototypeOf()`

设置对象的 `__proto__`，含义是给对象设置原型

```javascript
        obj1 = { name: "zoulam" }
        function a() { }
        Object.setPrototypeOf(obj1, a.prototype)
        console.log(obj1);
        // 效果一致且赋值的方式效率更高
        obj2 = { name: "lala" }
        function b() { }
        obj2.__proto__ = b.prototype
        console.log(obj2);
```

#### 对对象做出限制

`Object.freeze(obj)`，冻结对象，所有的操作都不会生效

`Object.fromEntries()` 键值对结构转化为对象，如`Map`、`Array`转化为```Obj``

`Object.getOwnPropertyDescriptor(obj, prop)` 获取**对象属性**的描述

| 描述 | 能力 |
| :--- | :--- |
| `value` | `obj.value` |
| `writable` | 可写 |
| `get` | `getter` |
| `set` | `setter` |
| `configurable`   **rable**结尾 | 可修改、删除 |
| `enumerable` | 可枚举 |

| 强描述 | 设置 | 效果 |
| :--- | :--- | :--- |
| `isFrozen()` | `freeze()` | 冻结（上方的的限制全是false） |
| `isExtensible()` | `preventExtensions()` | 扩展（无法添加属性） |
| `isSealed()` | `preventExtensions()` | 密封（永远是空对象）,无法封闭非空对象 |

> 下方的示范中声明了4个对象，分别是obj，obj0，obj1，obj2，后三个是用于设置强描述

```javascript
        const obj = {
            name: "zoulam",
            age: 18,
            [Symbol('sayName')]: function () { console.log('zoulam') },
            noEnumrable: 'go'
        }
        // 设置属性的权限,即配置
        Object.defineProperty(obj, 'noEnumrable', {
            value: "run change value",
            enumerable: false, // 不可枚举
            // get() { console.log('run getter'); return this.value },// 获取value的行为,与上方设置value冲突
            // set(newVal) { this.value = newVal; return newValue },// set和writable冲突，不管true还是false
            configurable: false, // 禁止删除修改
            writable: false// 禁止写入
        })
        console.log(obj.noEnumrable);    // value 打开"run change value"
        obj.noEnumrable = '18'
        console.log(obj.noEnumrable);    // value 打开"run change value"
        console.log('---------------------------------------------------------------------------');
        console.log('获取某个属性的描述: ', Object.getOwnPropertyDescriptor(obj, 'noEnumrable'));
        // 获取某个属性的描述:  {value: "run change value", writable: false, enumerable: false, configurable: false}
        console.log('获取全部描述:', Object.getOwnPropertyDescriptors(obj));
        // age: {value: 18, writable: true, enumerable: true, configurable: true}
        // name: {value: "zoulam", writable: true, enumerable: true, configurable: true}
        console.log('以数组格式获取全部key（不含Symbolkey）: ', Object.getOwnPropertyNames(obj));  //  获取的数组类型的key，包含不可枚举的,keys返回的是可枚举的
        //  ["name", "age", "noEnumrable" ]
        console.log('以数组格式获取可枚举key（不含Symbolkey）: ', Object.keys(obj));  //  获取的数组类型的key，包含不可枚举的,keys返回的是可枚举的
        //  ["name", "age"]
        console.log(Object.prototype.toString.call(Object.getOwnPropertyNames(obj)));
        // [object Array]
        console.log('以数组格式获取全部Symbolkey: ', Object.getOwnPropertySymbols(obj));
        // [Symbol(sayName)]
        console.log('获取__proto__: ', Object.getPrototypeOf(obj));// 与下面的等价
        console.log(obj.__proto__);
        console.log('比较两个值是否相等', Object.is(1, 1));// true 效果类似于 ==
        console.log('-----------------------------------扩展----------------------------------------');
        console.log('对象是否可扩展: ', Object.isExtensible(obj));// true 是否可扩展，即是否能添加属性
        let obj0 = { name: "momo" }
        Object.preventExtensions(obj0)
        console.log('对象是否可扩展: ', Object.isExtensible(obj0));// false 是否可扩展，即是否能添加属性
        console.log('-----------------------------------冻结----------------------------------------');
        console.log('对象是否冻结: ', Object.isFrozen(obj));// false 是否被冻结
        let obj1 = { name: "nana" }
        Object.freeze(obj1)
        obj1.name = "chuchu"
        console.log('对象是否冻结: ', Object.isFrozen(obj1));// true
        console.log(obj1);// object.html:53 {name: "nana"} 修改失败
        console.log('-----------------------------------密封----------------------------------------');
        Object.preventExtensions(obj)// 密封对象，只对空对象生效，此处密封失败
        console.log('对象是否被密封: ', Object.isSealed(obj));// false 是否被密封
        let obj2 = {}
        Object.preventExtensions(obj2)
        console.log('对象是否被密封: ', Object.isSealed(obj2));// true
        obj2.name = "lala"
        console.log(obj2);// {}
```

![obj](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201125004347846.png)

`Object.defineProperties()`

```javascript
const testObj = {}
Object.defineProperties(testObj, {
    'property1': {
        value: true,
        writable: true
    },
    'property2': {
        value: 'Hello',
        writable: false
    }
    // ...
});
```

#### es6的语法糖

```javascript
        class DefineProperty {
            constructor(name) {
                this.name = name
            }
            get Name() {
                console.log('获取的时候被劫持了');
                return this.name
            }
            set Name(newName) {
                console.log('设置的时候被劫持了');
                this.name = newName
            }
        }

        let data = new DefineProperty('zoulam')
        console.log(data.name);
        // zoulam
        console.log(data.Name);
        // 获取的时候被劫持了
        // zoulam
        data.Name = 'lala'
        // 设置的时候被劫持了
```

### 对象原型方法

`Object.prototype.hasOwnProperty()` 查看对象的属性和方法是否挂载在对象上而不是原型上

```javascript
        class Animal {
            type = "animal"
        }

        class Dog {
            name = "lala"
        }

        Dog.prototype.isStrong = true
        const obj1 = new Dog()
        obj1.age = 18
        obj1.__proto__.sex = 'female'
        console.log(obj1);

        // 只要不是this上的都是false
        console.log(obj1.hasOwnProperty('type'));// false
        console.log(obj1.hasOwnProperty('name'));// true
        console.log(obj1.hasOwnProperty('age'));// true
        console.log(obj1.hasOwnProperty('sex'));// false
        console.log(obj1.hasOwnProperty('isStrong'));// false
```

`Object.prototype.valueOf()` 原始值的包装类 =&gt; 原始值

[Symbol.toPrimitive【原始值】](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol/toPrimitive)

```javascript
if([] == false){ // false 不是对象类型 []是对象类型会隐式调用 toString()
    console.log('run')
}
```

### 增删查改

```javascript
let obj = {
    name: 'zoulam',
    error: ''
}
console.log(obj.name);// zoulam 查
console.log(name in obj);// true 查
obj.age = 18 // 增
delete obj.error // 删
obj.name = 'lala'// 改
console.log(obj); // {name: "lala", age: 18}
```

## 12、dom

> DOM是针对HMTL和XML文档的一个API。DOM描绘了一个层次节点树，允许开发人员添加、移除和修改页面的一部分。DOM脱胎于Netscape及微软公司创造的DHTML\(动态HTML\)，但现在它以及成为表现和操作页面标记的真正的跨平台、语言中立的方式。

```javascript
类型（typescript中用上）
HTMLCollection dom数组
HTMLElement 基类
HTMLDIVElement div节点
```

```javascript
// dom节点是实例化 HMTLXxxElement
<body>
    <div id="div"></div>
    <script>
        let node = document.getElementById('div');
        console.log(node instanceof HTMLDivElement);// true
        console.log(Object.prototype.toString.call(node));// [object HTMLDivElement]
    </script>
</body>
```

### id可以直接获取

```javascript
<body>
    <div id="test"></div>
    <script>
        console.log(test);
    </script>
</body>
```

### 标签选择器

获取单个、获取`nodeList`即伪数组

`css`语法选择器 `querySelector` 选择符合条件的第一个节点，`querySelectorAll`选择全部满足条件的节点

```javascript
document.querySelector('button#tabbar')// 按钮且id是tabbar的节点
document.querySelector('button.lala')// 按钮带有lala类名的节点
document.querySelectorAll('ul li')// ul下的全部li节点
```

### DOM节点的增删改

| 操作  （A为父节点bc为兄弟子节点，D为document的简写） | 效果 |
| :--- | :--- |
| `D.write()` |  |
| `D.createElement('div')`  `D.createDocumentFragment()` | 前者在原文档创建，后者在空白文档【内存空间上】创建，后续插入文档碎片的概念也是DOM操作的优化点 |
| `A.appendChild()` |  |
| `D.createTextNode()` |  |
| `A.replaceChild(b, c)` | **b换c** |
| `A.insertBefore(b, c)` | **b插在c前面** |
| `node.clone()` | 含参数，默认是是`false` ，`true`是深拷贝，即包含子节点 |
| `removeChild()` |  |

### 行内属性的获取和设置，节点包含的（常用）属性

通常会使用 `attributes` 获取全部属性\(**包含自定义属性**，以类数组的形式\)，然后再具体操作

```javascript
      // 拆分键值对的方式，解构出来的只有自定义属性和类名
    let attrs = oName.attributes;
    // 单独获取类名
    console.log(attrs.class.value)
        [...attrs].forEach(attr => {
            // 解构获取键值 name 和 value 如有必要可以进行重命名
            let { name, value : expr } = attr;
            console.log(name, expr);
        })
```

`innerText（不包含子元素的文本） textContent（包含子元素的文本） innerHTML outerHTML value(表单元素特有) classList（类名列表）`

#### 增删改attribute

`element.getAttribute(name)`

`element.setAttribute(name, value)`

`element.removeAttribute(keyName)`

```text
setAttribute 是覆盖类型的添加属性，即将原有的属性删除，再添加
如
node.setAttribute('class', 'momo');
等效于 node.className = 'momo'
还有不覆盖类型的，使用字符串拼接
    node.className += ' momo'
```

#### [attribute 和 property的区别](https://www.cnblogs.com/lmjZone/p/8760232.html)

>  property是DOM中的属性，是JavaScript里的对象；
> attribute是HTML标签上的特性，它的值只能够是字符串；
>
> **attributes是属于property的一个子集**

### 父子、兄弟节点

三个判定属性

`nodeType` 返回`number` **下面的more**有对应的 `number-nodeType`的具体映射表

`nodeName` 大写的节点名称

`nodeValue` 只读，如果是`null`也不可以设置

`style` 格式如下

```javascript
node.style.cssText += `width:100px;height:15px;background-color:orange;`
node.style.borderBottom = "10px" // 此处的borderBottom遵循驼峰命名
```

```javascript
    <div class="try" key="2"></div>
    <script>
        let node = document.getElementsByClassName('try')[0];
        let value = node.attributes.getNamedItem('key'); // 对象类型的 key="2"
        let realValue = value.nodeValue; // string 类型的2
    </script>
```

[more](https://developer.mozilla.org/zh-CN/docs/Web/API/Node)

#### 父亲节点

`parentNode` `parentElmenet`

基本一致，唯一的区别就是parentNode是能够识别 `#doucument`

```javascript
    <ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
        <li>5</li>
    </ul>
    <script>
        let node = document.getElementsByTagName('li')[0];
        console.log(node.parentNode.parentNode.parentNode.parentNode); // #doucmenet
        console.log(node.parentElement.parentElement.parentElement.parentElement);// null
    </script>
```

#### 子节点列表

以下两个会出无法跳过**空格、换行**，即会读取文本节点：`childNodes` `firstChild` `lastChild`

只读取元素节点： `children` `firstElementChild` `lastElementChild`

#### 兄弟节点

`nextSibling` 下一个兄弟

`previousSibling` 上一个兄弟

### 自定义数据

`data-*`

`node.dataset`

复杂数据

`node.getAttribute("data-*")`

`JSON.parse(node.getAttribute('data-*'))` 解析成 `JSON`对象 **注意里面要用单引号就好了**

```javascript
    <div class="try" data-set="[1,2,23,3,5]" data-try="{'name':'lala'}"></div>
    <script>
        let node = document.getElementsByClassName('try')[0];
        let dataSet = node.dataset.set
        let dataTry = node.dataset.try
        console.log(dataSet);
        console.log(dataTry);
        let nums = JSON.parse(node.getAttribute('data-set')) // 数组类型的[1,2,23,3,5]
        console.log(Object.prototype.toString.call(nums));// [object Array]
    </script>
```

### classList

> 打印信息如下图，包含了大量函数，可以操作类名

```javascript
<body>
    <div id="node" class="div test get try node child"></div>
    <script>
        const node = document.getElementById('node');
        console.log(node.classList);
        console.log(Object.prototype.toString.call(node.classList)); // [object DOMTokenList]
    </script>
</body>
```

![classList](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201122222323753.png)

### 节点位置信息

**记忆方式大的，等于两个小的的和，忘记的时候可以答应出来**

滚动到底时满足等式： `scrollHeight - scrollTop = clientHeight`

|  | 偏移（基本等于你设置） | 滚动 | 客户端 |
| :--- | :--- | :--- | :--- |
| 描述 | **（content+padding+border）**或**（width）** | （无溢出时与client相等）（溢出时：client+溢出大小） | （**content+padding**）或（**width-border**） |
|  | offsetWidth | scrollWidth | clientWidth**（不含border、margin、滚动条）** |
|  | offsetHeight | scrollHeight | clientHeight |
|  | offsetLeft **（外边距距离父元素的距离）** | scrollLeft**（可写、滚动条位置）** | clientLeft**（边框非transport才有效）** |
|  | offsetTop | scrollTop | clientTop |

![&#x793A;&#x4F8B;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/201211191754284481.png)

#### 鼠标事件位置

| offset | client | page | screen（） |
| :--- | :--- | :--- | :--- |
| 实验性api（鼠标点击位置与元素padding的距离） | （滚动和缩放都不会改变原点位置） | （页面无滚动条时与client相同） | （全屏时与client相同） |
| offsetX offsetY |  |  |  |

[看这篇文章把](https://juejin.im/post/6844903854157332487)

## 常用事件

```javascript
// 时间的命名都是 onxxx的格式,语义是当什么的时候触发|执行
    onclick = function(){} // 当鼠标点击的时候执行
```

|  | 共同点 | 不同点 | 含义 |
| :--- | :--- | :--- | :--- |
| ```event.currentTarget`` | 都是dom节点 | 等价于`this`，当事件绑定在父元素，点击子元素时指向**父元素** | 事件**绑定**的节点 |
| `event.target` |  | 当事件绑定在父元素，点击子元素时指向**子元素** | 事件**触发**的节点 |

```javascript
<body>
    <ul id="box">
        <Li id="apple">苹果</Li>
    </ul>
</body>
<script>
    var box = document.getElementById('box');
    box.onclick = function (e) {
        console.log(e.target);// <li id="apple">苹果</li>
        console.log(e.currentTarget);// <ul id="box">...</ul>
        console.log(e.currentTarget === this); // true
    }
</script>
```

**MouseEvent** [more](https://developer.mozilla.org/zh-CN/docs/Web/API/MouseEvent/MouseEvent)

meta:中文意思是可变化的意思

```javascript
<body>
    <div id="try"></div>
    <script>
        let node = document.getElementById('try');
        node.onclick = function (e) {
            if (e.altKey) {
                console.log(`你点击了alt键和鼠标左键`);
            }
        }
    </script>
</body>
```

`click`

`MouseEvent.shiftKey`

`MouseEvent.ctrlKey`

`MouseEvent.altKey`

`MouseEvent.metaKey【win或mac】`

`dbclick`

`mouseover` `mouseout` 传递到子元素

`mouseenter` `mouseleave` 不传递到子元素

`mousemove`

`mousedown` 按下未松开

`mouseup` 松开

**KeyboardEvent**[more](https://developer.mozilla.org/zh-CN/docs/Web/API/KeyboardEvent/KeyboardEvent)

`keyup`

`keydown` `event.keyCode` \(读取不区分大小写的字母和 `shift`等快捷键\)

`keypress` `event.charCode` （读取数组和区分大小写的字母）

**HTMLFormEvent** [more](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLFormElement)

`blur` `focus` `select` `change`

这两个跟按钮的效果是一样的：`submit` `reset`

### 事件冒泡和捕获

`addEventListner('eventName', callback, false)`

第三个参数默认是false，事件冒泡

填入true就是事件捕获

冒泡：点击**子节点**，从子节点向父节点传递，如点击`p`标签直至 html文档。 **p -&gt; div -&gt; body -&gt; html -&gt; document**

捕获：点击**子节点**，从父节点向子节点传递，直至当前点击节点。

### **阻止事件**

`event.preventDefault()` 阻止默认事件（如：点击超链接会跳转超链接），但不会阻塞后续事件；

`event.stopPropagation【传播】()` 阻止事件冒泡或者捕获的传播，**而不能阻止当前事件**；

`event.stopImmediate【立即】Propagation()` 阻止后续相同类型的事件执行。

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

### 动态类名

>  场景：点击某一个按钮让他变成高亮，之前高亮的按钮失去高亮。
>
>  使用事件委托获取被点击的节点，用`index`保存之前高亮节点的下标，用偷来的数组方法 `indexOf`来获取当前高亮的下标并更新 `index`

```javascript
<body>
    <ul class="tabbar">
        <span class="highlight">首页</span>
        <span>导航</span>
        <span>个人中心</span>
    </ul>
    <script>
        const tabber = document.getElementsByClassName('tabbar')[0]
        const tabberChilds = tabber.getElementsByTagName('span')
        let index = 0
        // 事件委托
        tabber.onclick = (event) => {
            // 兼容
            // const e = event || window.event;
            // const tar = e.target || e.srcElement;
            let tar = event.target
            tar.tagName.toLowerCase() === 'span' && colorChange(tar)
        }

        const colorChange = function (target) {
            tabberChilds[index].className = '' // 更准确的是使用classList的方法删除指定类名 
            target.className += 'highlight'
            index = [].indexOf.call(tabberChilds, target)
        }
    </script>
</body>
```

```javascript
<body>
    <ul class="tabbar">
        <span class="highlight">首页</span>
        <span>导航</span>
        <span>个人中心</span>
    </ul>
    <script>
        const tabber = document.getElementsByClassName('tabbar')[0]
        const tabberChilds = tabber.getElementsByTagName('span')
        let index = 0
        tabber.onclick = function (e) {
            target = e.target
            if (target.tagName.toLowerCase() == 'span') {
                tabberChilds[index].className = ''
                target.className += 'highlight'
                index = [].indexOf.call(tabberChilds, target)
            }
        }
    </script>
</body>
```

### **MutationOberserver**

`mutation` 的中文意思是突变；变化的意思。

[MutationObereserver-mdn](https://developer.mozilla.org/zh-CN/docs/Web/API/MutationObserver)

### 常见问题

`onload` 和 `DOMContentLoaded`的区别

`DOMContentLoaded` ：【含义是dom结构加载完成】页面的静态资源加载完成前

中间夹着静态资源

`onload`：页面全部内容加载完成

## 13、BOM

**主要了解两个与路由有关的api**

### screen

![screen](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201101231755543.png)

### [history](https://developer.mozilla.org/zh-CN/docs/Web/API/History)

![history](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201101231904456.png)

> 操作用户访问历史的行为

### location

![location](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201101232202247.png)

> 拼接url

```javascript
location.href = 'xx.html' // 会让跳转到xx.html页面下
```



## 14、LocalStorage

| localstorage（IE8以下不兼容） | cookie | sessionstorage\(服务端缓存\) |
| :--- | :--- | :--- |
| 永久储存 | 可以设置过期时间。一般情况下，每个域名最多50条 |  |
| 大小 5M | 最大可以存4KB | 容量极小 |
| 只能存储字符串 |  | 有效时间极短，页面关闭可能就销毁了 |

`localStorage.setItem(key, value)`

当然也有赋值的方式，因为localStorage是一个全局变量

`localStorage.key = value`

`localStorage.getItem(key)` `return value`

`localStorage.key`  `localStorage['key']` 使用变量的方式获取

`localStorage.remove(key)`

## 15、Reflect and Proxy

>  元编程：简单的理解就是代码操纵代码，更深入点就是**编写代码操作代码本身在，执行时完成本应在编译时的工作**。
>
>  场景：原本构建的`xx_price`属性只是数字格式的字符串，现在希望在前面加上 `￥`符号。
>
>  1、编写一个函数，对`xx_price`进行正则匹配，修改属性值，添加`￥`，
>
>  这一步完成后还有一个，那就是后续添加的价格仍旧没有 `￥`需要再次执行改函数
>
>  2、反射，在反射的`set(){}`方法内编写上述1的函数，代码如下
>
>  **注**：此处是对 `proxy`处理才会触发陷阱

```javascript
        const product = {
            pen_type: "pencil",
            pen_price: "1",
            paper_type: "A4Paper",
            paper_price: "0.2",
        }
        const handler = {
            set(oTarget, sKey, vValue) {
                if ((/(.*)_price$/i).test(sKey)) {
                    oTarget[sKey] = '￥' + vValue
                    return
                }
                oTarget[sKey] = vValue
            }
        }
        const proxy = new Proxy(product, handler)
        proxy.box_type = 'normal box'
        proxy.box_Price = '15'
        console.log(proxy);
        console.log(product);
```

>  反射：假设A语言能对B语言进行元编程，那么A就是B的反射。JavaScript的`Reflect、Proxy`能对JavaScript进行元编程，那么JavaScript就是JavaScript的反射。
>
>  陷阱（traps）：走到某处就会踩到的东西，如获取值就会踩到`get(){}`陷阱，使用in语法就会踩到 `has(){}`陷阱。

```javascript
const p = new Proxy(原始对象, （handler）捕捉器[是一个对象，里面有很多函数])
```

>  `handler`可以作为校验器，也可以避免出现`undefined`设置默认值，

```javascript
    // 语义有就返回正常值，没有就返回37，即可以避免出现undefined的麻烦，添加默认值
    const handler = {
        get: function (obj, prop) {
            return prop in obj ? obj[prop] : 37;
        }
    };

    const p = new Proxy({}, handler);
    p.a = 1;
    p.b = undefined;

    console.log(p.a, p.b);      // 1, undefined
    console.log('c' in p, p.c); // false, 37
```

```javascript
    const product = {
        pen_type: "pencil",
    }
    const handler = {
        has(oTarget, sKey) {
            console.log('hi man you traps has');
            return sKey in oTarget
        }
    }
    const p = new Proxy(product, handler)
    const res = 'pen_type' in p // 'hi man you traps has'
    console.log(res);// true
    const res2 = 'pen_price' in p // 'hi man you traps has'
    console.log(res2);// false
```

通常传入三到四个参数

```text
get(oTarget, sKey, vValue / oDesc, receiver)
oTarget 被代理对象
sKey 属性名
vValue 属性值，oDesc 属性的描述
receiver 代理后的对象
```

`handler.getPrototypeOf()`

Object.getPrototypeOf 方法的捕捉器。

`handler.setPrototypeOf()`

Object.setPrototypeOf 方法的捕捉器。

`handler.isExtensible()`

Object.isExtensible 方法的捕捉器。

`handler.preventExtensions()`

Object.preventExtensions 方法的捕捉器。

`handler.getOwnPropertyDescriptor()`

Object.getOwnPropertyDescriptor 方法的捕捉器。

`handler.defineProperty()`

Object.defineProperty 方法的捕捉器。

`handler.has()`

in 操作符的捕捉器。

`handler.get()`

属性读取操作的捕捉器。

`handler.set()`

属性设置操作的捕捉器。

`handler.deleteProperty()`

delete 操作符的捕捉器。

`handler.ownKeys()`

Object.getOwnPropertyNames 方法和 Object.getOwnPropertySymbols 方法的捕捉器。

`handler.apply()`

函数调用操作的捕捉器。

`handler.construct()`

new 操作符的捕捉器。

### 一个疑问？

>  明明对象构造器上就有了一部分`Reflect`的方法，为什么还要单独开一个构造器而不是在原来的 `Object`构造器上添加新方法，`Reflect`与`Object`的同类方法比又有什么区别。
>
>  `Reflect`的函数有返回值，能够在`handler`中做出更好的处理。

```javascript
let baseHander = {
        get(target, key, receiver) {
            // target目标值，key代理对象的key，receriver proxy后的对象

            let result = Reflect.get(target, key, receiver);
            // console.log('get value');

            // 收集依赖（订阅）
            track(target, key);

            return isObject(result) ? reactive(result) : result;
            // return target[key];
        },
        set(target, key, value, receiver) {
            // 修改数组长度时需要屏蔽，不然一次修改就要变更两次视图
            let hadKey = hasOwn(target, key);
            let oldVal = target[key];
            let res = Reflect.set(target, key, value, receiver);
            if (!hadKey) {
                trigger(target, 'add', key);

            } else if (oldVal !== value) {// 表示属性已经更改过了
                trigger(target, 'set', key);
            }
            //如果设置没成功（writeable:false）,也不会通知用户，用反射就能解决这个问题
            // console.log('set value');
            // target[key] = value;
            return res;
        },
        deleteProperty(target, key) {
            console.log('delete value');
            return Reflect.deleteProperty(target, key)
        }
    };
    let observed = new Proxy(target, baseHander);
    toProxy.set(target, observed)
    toRaw.set(observed, target)
    return observed;
}
```

