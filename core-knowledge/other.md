# 其他

## 数组扁平化

> 需要深层考虑元素的内容，可能会有对象等

### reduce

```javascript
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

## 数组去重，获取最大数，排序（升序，降序，随机）

```javascript
let nums = [1, 2, 3, 4, 4, 5, 5, 1, 1, 1, 1]
let notRepeatNums = [...new Set(nums)]
let maxNum = Math.max(...nums)
// sort是原地操作，每次都需要打印
let randomNums = nums.sort(() => (Math.random() * 2) - 1)// [0,1) => [-1,1)
console.log(randomNums);
let lowToUp = nums.sort((a, b) => a - b)
console.log(lowToUp);
let upToLow = nums.sort((a, b) => b - a)
console.log(upToLow);
```

## 深拷贝

>  深拷贝是浅拷贝的对应说法，浅拷贝是拷贝地址值，拷贝后的新对象是地址的引用，就会出现一个问题，修改新对象的值，旧对象的值也会发生变化。
>
>  所以深拷贝是对存在堆空间的值进行深层次的遍历，拷贝原始值。
>
>  更深一层还需要考虑，对象的`description`，即对象是否可枚举，可写，冻结、封闭、扩展等

### JSONapi

```javascript
 let newObj = JSON.parse(JSON.stringify(oldObj))// 丢失undefined和function
```

```javascript
class B {
    name = 'zoulam'
}
class A extends B {
    nums = [1, 3, 4, 5]
    age = 18
    child = {
        name: "lala",
        age: 1
    }
    getName() {
        console.log(this.name)
    }
    female = undefined
}

let oldObj = new A()

let newObj = JSON.parse(JSON.stringify(oldObj))
console.log(newObj);// {name: "zoulam", nums: Array(4), age: 18, child: {…}}
```

### 手写

>  正则的`new`方式，`/test$/g` 为例， `regexp.source == test$` `regexp.flags== g`
>
> 1、创建容器并缓存属性值
>
> 2、普通值直接赋值，对象和数组递归
>
> 3、递归的时候终止递归的条件是普通值和缓存过的，使用`weakMap`是容易被垃圾挥手
>
> 4、函数改变this指向，同时别忘记挂上参数
>
> 5、其他对象就使用`new`语法创建

```javascript
let obj = {
    name: 'zoulam',
    age: 18,
    child: {
        name: 'lala',
        age: 1
    },
    getName() {
        return this.name
    },
    female: undefined,
    income: [1, 2, 3, 45, 76],
    createAt: new Date,
    Symbol: Symbol(15),
    regexp: /test$/g
}

function deepCopy(obj, cache = new WeakMap()) {
    // 遍历属性的各种的规则和终止递归的条件
    if (!obj instanceof Object) return obj
    if (cache.get(obj)) return cache.get(obj)
    if (obj instanceof Function) {
        return function () {
            obj.apply(this, arguments)
        }
    }
    if (obj instanceof Date) return new Date(obj)
    if (obj instanceof RegExp) return new RegExp(obj.source, obj.flags)
    // 处理数组情况，数组的内容跟对象一样复杂，里面可以包含各种内容
    const res = Array.isArray(obj) ? [] : {}
    cache.set(obj, res)

    // 遍历对象属性，并递归
    Object.keys(obj).forEach((key) => {
        if (obj[key] instanceof Object) {
            res[key] = deepCopy(obj[key], cache)
        } else {
            res[key] = obj[key]
        }
    });
    return res
}
```

![code-review](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201125171721084.png)

```javascript
function deepCopy(obj, cache = new WeakMap()) {
    if (!obj instanceof Object) return obj
    if (cache.get(obj)) return cache.get(obj)
    if (obj instanceof Function) {
        return function () {
            obj.apply(this, arguments)
        }
    }
    // if(obj instanceof ) return new
    // if(obj instanceof ) return new
    // if(obj instanceof ) return new
    // if(obj instanceof ) return new
    // if(obj instanceof ) return new

    let res = Array.isArray(obj) ? [] : {}
    cache.set(obj, cache)
    Object.keys(obj).forEach((key) => {
        if (obj[key] instanceof Object) {
            res[key] = deepCopy(obj[key], cache)
        } else {
            res[key] = obj[key]
        }
    })
    return res
}
```

# 最简版深拷贝

```JavaScript
function deepClone(obj = {}) {
    if (typeof obj !== 'object' || object == null) {
        return obj
    }
    
    let res
    if (Array.isArray(obj)) {
        res = []
    } else {
		res = {}
    }

	for (let key in obj) {
		if(obj.hasOwnProperty(key)) {
            res[key] = deepClone(obj[key])
        }
	}
    return res
}



```

## debounce

> **防抖**，理解为回城被打断，回城过程中被打断就需要重新计时才能回城成功
>
>  1、高阶函数，返回值是函数（返回值也是函数，即改变`this`和注入参数）；2、参数一为函数（回城），参数二为回城时间（填入默认）
>
>  使用场景：输入验证，输入补全，下拉加载，上拉刷新，`resize`等`DOM`操作，避免频繁触发，造成性能呢问题。

```javascript
function debounce(func, delay = 1000) {
    let timeout = null
    return function () {
        let context = this
        let args = arguments
        if (timeout) clearTimeout(timeout)
        timeout = setTimeout(function () {
            func.apply(context, args)
        }, delay)
    }
}
```

## throttle

> **节流**，理解为技能cd，函数理解成技能，技能在施放之后未冷却的情况下都不能重新施放。
>
>  1、高阶函数（返回值也是函数，即改变`this`和注入参数），返回值是函数；2、参数一为函数（技能），参数二为cd（填入默认）
>
>  使用场景：表单提交，发送`http`、`ajax`请求等
>
>  下面的示范是时间戳版本【即首次不节流】,定时器版本【首次节流】，
>
>  进一步优化合并两种写法，填入第三个参数，`isTroggle`让用户自定义首次是否节流。

```javascript
function throttle(func, delay = 1000) {
    let previous = 0
    return function (...args) {
        const context = this
        const now = Date.now()
        if (now - previous >= delay) {
            func.apply(context, args)
            previous = now
        }
    }
}
```

```javascript
function throttle(func, delay = 2000) {
    let timeout = null;
    return function (...args) {
        let context = this;
        if (!timeout) {
            timeout = setTimeout(function () {
                timeout = null;
                func.apply(context, args)
            }, delay)
        }
    }
}
```

