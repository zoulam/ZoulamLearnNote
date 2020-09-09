---
description: 温馨提示，本文是以JavaScript为主要语言写的数据结构与算法，以及刷leetcode的记录
---

# JavaScript刷题前置知识

## 熟记常用API 

### `Math` 

`Math.max(value)`

 `Math.min(value)` 

`Math.floor(value)` 向下取整(即取更小的值)  45.9=>45 -45.9=>-46

 `Math.sqrt(value)` 

`Math.pow(底数、指数)`  或  `底数 ** 指数`

`Math.round(value)`  四舍五入

`Math.trunc(value) ` 抹零

`Math.abs(value)`

 `1e9+7` 常用于取模的超大数

`parseFloat(String)` `parseInt(String,进制格式默认值是10)` `Float.fixed(保留小数的位数)`

### `String`

`replace(选定内容, 替换内容)`  选定内容常使用 正则填充

`trim()` 

`trimRight()` 

`trimeLeft()`

`split(分割值)` 

 `slice(startIndex, endIndex + 1)`

`indexOf()`

`includes()`

`charAt()`  不常用  一般使用语法： `String[index]`

`charCodeAt()`

`toLowerCase()`

`toUpperCase()`

###  `Array` 

原地操作

​	`push()` `unshitft()`

​	`pop()` `shift()`

​	`reserve()`  `sort()`

​	`splice(startIndex, deleteLength, [...addValue])` 

​			`deleteLength = 0` 时插入  其他从 startIndex位置开始删除

​	`forEach((el, index, array)=>{do something}, this)` 

非原地操作

​	`slice()` `join()` `toString()` `fill(替换内容, startIndex ,endIndex + 1)`

​	`concat(a1, a2, a3, ……)` 将后面的数组合并在前面的数组

​	`map((el, index, array)=>{do something}, this)`

​	`filter((el, index, array)=>,this)`

​		表达式中的返回值为true就push进入新数组，false则被过滤

​	`find(el => el >10 )`

​		与`filter`的逻辑相同，是短路操作，即只返回一次值

​	`reduce((start, el, index, array)=> start + el,init)`

​		这里是一个简单的累加器 `start`为上一次执行的`return`值，`init`是首次进入函数的初始值

​	`indexOf(value)`  `value`存在返回`0` 否则返回`-1`

​	`includes(value)` 返回值的下标

​	关于迭代 使用 `for(let value of Array) {}` 才能保证顺序

### `RegExp`

略

###  `Map`

`set()`

`get()` 取出来查看，并不会删除

`has()`

`clear()`

`delete()`

`size`

###  `Set`

`add()`

`get()`

`has()`

`size` 是去重之后的尺寸，而不是填入的长度

`clear()`



了解常用数据结构：队列、优先级队列（`push`、`shift`）、栈(`push`、`pop`）、链表、双向链表、环形链表、二叉树

二分查找模板、双指针模板、位运算使用场景