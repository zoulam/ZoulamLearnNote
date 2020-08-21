# 常用API

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

## 数学

## 数组

## 日期

## 时间

## dom（略）