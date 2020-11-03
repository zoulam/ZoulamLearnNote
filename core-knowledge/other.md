# 其他

## 数组扁平化

>  需要深层考虑元素的内容，可能会有对象等

### reduce

```JavaScript
let nums = [1, [2, 3, [4, 5]]]
function flatten(arr) {
    return arr.reduce((result, item) => {
        return result.concat(Array.isArray(item) ? flatten(item) : item);
    }, [])
}
let ans1 = flatten(nums)
console.log(ans1);
```



### 库函数flat

```javascript
function flatten2(arr) {
    function isFlatten(arr) {
        for (let i = 0; i < arr.length; i++) {
            if (Array.isArray(arr[i])) return false
        }
        return true
    }
    while (!isFlatten(arr)) {
        arr = arr.flat()
    }
    return arr
}
let nums2 = [1, [2, 3, [4, 5]]]
console.log(flatten2(nums2));
```

### 扩展运算符

其中用到了一个测试函数 `some`，相当于是遍历元素只要有一个是正确的就返回`true`

```javascript
function flatten3(arr) {
    while (arr.some(item => Array.isArray(item))) {
        arr = [].concat(...arr);
    }
    return arr;
}

let nums3 = [1, [2, 3, [4, 5]]]
console.log(flatten3(nums3));
```



## 数组去重