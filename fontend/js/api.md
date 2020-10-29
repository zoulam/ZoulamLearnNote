---
description: 用于速记，平时使用查文档即可
---

# \[js\]API

两类API

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

## WeakSet

## dom（略）

