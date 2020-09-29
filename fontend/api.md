---
description: 用于速记，平时使用查文档即可
---

# \[js\]API

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
    'slice': Array.prototype.slice
}

// 入侵式添加
let nums1 = obj.slice(0, 2)
console.log(nums1);// [ 'zoulam', 'luluxi' ]
// 非入侵式添加
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

`split(',')` return Array

`charCodeAt(index)` `charAt(index)`

`slice(startIndex,endIndex)`

`trim()` `trimRight()` `trimLeft()`

`replace(oldString, newString)`

`match()`

`toLowerCase()` `toUpperCase()`

## 数学

## 数组

`push(...agrs)` `unshift(...args)`

`pop()` `shift()`

`splice(startIndex, length, ...spliceContent)`

`reserve()` `sort((a, b)=> a - b)`

`concat()`

`filter((element, index, array)=>{}, this)` true push newArr

`forEach((element, index, array)=>{ element doSomething})` oldArray

`map((element, index, array)=>{element doSomething})`newArr

`reduce((first, index, Array)=>{},first)`

`join(',')` === `toString()` return String

`slice(startIndex,endIndex)` 左闭右开

## 日期

`Data().now`

## Map

`let map = new Map()`

`size`

> 参数为不填或者填入key

`has(key)`

`set([key,value])`

`get(key)`

`delete(key)`

`clear()`

`keys()` 可迭代的`key`

> `for (let key of map){`
>
> `}`

`values()` 可迭代的`value`

> `for (let [key, value] of map){`
>
> `}`

`entries()` 可迭代的`['key', 'value']`

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

## Set

`let set = new Set([1, 2, 3, '3'])`

`add()`

`clear()`

`delete()`

`has()`

`forEach()`

`values()`

`entries()`

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
let set = new Set([1, 2, 3, '3', 3, 3])
```

## WeakMap

## WeakSet

## dom（略）

