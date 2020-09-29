# \[js\]函数、函数式编程（入门）

## 函数的多种写法

### 标准函数

```javascript
function test1() { }
let test2 = function () { }
```

### 函数简写（对象\|\|class内）

```javascript
let obj = {
    test3() { }
}

// 在class内只能这样声明函数
class Test {
    test3() { }
}
```

### 箭头函数

```javascript
// 箭头函数
() => { } // 标准声明

a => c // 当且仅当只有一个参数：a 且执行语句只有c
```

## 返回值

### 默认返回`undefined`

```javascript
function test1(){}
console.log(test1());//默认返回值是undefined
```

### 构造函数默认返回`this`

```javascript
function Compute() {
    this.add = function (a, b) {
        console.log(a + b);
    }
}

var compute = new Compute();
console.log(compute);// Compute { add: [Function] }
```

### 返回值类型上的区别

> 基本数据类型不会覆盖`this`,会被`this`覆盖,引用数据类型**会覆盖**`this`

```javascript
function Compute() {
    this.add = function (a, b) {
        console.log(a + b);
    }
    //return this;//会被return引用数据覆盖报错
    //return [];//VM66:9 Uncaught TypeError: compute.add is not a function
    //at <anonymous>:9:9
}

var compute = new Compute();
compute.add(1, 2);
```

## 参数（arguments、rest）

> arguments：是**对象类型**（伪数组），即无法调用数组方法（箭头函数中没有实现arguments对象）
>
> rest：是**数组类型**，可以调用丰富的数组方法

```javascript
// 默认值
function test(a) {
    a = arguments[0] || 100;
    console.log(a + 10);
}
test();//110
test(10);//20

// 新语法
function test(a = 100) {
    console.log(a + 10);
}
test();//110
test(10);//20



// rest
// args是个标识符，可以按需命名
function test2(...args) {
    console.log(args);// [ 1, 2, 3, 4 ]
    console.log(args[2]);// 3
}

test2(1, 2, 3, 4);
```

### 扩展运算符实现多参数

```javascript
// 扩展运算符
// 传入多个参数
function test(...args) {
    console.log(arguments);
    console.log(args);
}

let name = ["luluxi", "lala", "momo"];
test(...name);//此处是传入三个字符串参数，而不是一个数组参数
//[Arguments] { '0': 'luluxi', '1': 'lala', '2': 'momo' }
//[ 'luluxi', 'lala', 'momo' ]

test(name);
//[Arguments] { '0': [ 'luluxi', 'lala', 'momo' ] }
//[ [ 'luluxi', 'lala', 'momo' ] ]
```

## 立即执行函数\(IIFE\)

> 立即执行函数\(Immediately-invoked function expression\)的优点是执行完之后就销毁，不再消耗**命名空间\(即右单独的词法作用域\)和内存**，**可配合闭包实现单文件的模块化开发**。

```javascript
(function () {}());//w3c 建议写法，但可读性较差

(function add() {})();

// add2()//ReferenceError: add2 is not defined
// 立即执行函数拥有执行完就销毁，不污染全局命名空间的优点
// 即不需要函数名，写上也无任意义

// 插件的写法
; (function () { })()
; (function () { })()
; (function () { })()

// 尾部括号跟执行普通函数一样，是可以传输实参的
// 可以使用变量接收返回值，即可以只用一个变量命名的空间完成函数功能

let result = (function (a, b) {
    return a + b;
})(1, 2);

console.log(result);//3
```

### 奇葩写法

> 原理是**表达式+立即执行符号**

```javascript
// 立即执行函数原理 奇葩的立即执行函数
// expression + iffe symbol 即：表达式加上立即执行符号();

//在函数前添上 + - ！ || && 括号包裹
+ function test3(){
    console.log(1);
}();

1 && function test3(){
    console.log(1);
}();
```

### 模块化开发示例

```javascript
// 立即执行函数配上闭包，实现单文件的模块化开发(即使用最少的变量名来实现复杂功能)

// 原型继承函数
var inherit = (function () {
    var Buffer = function () { }
    return function (Target, Origin) {
        Buffer.prototype = Origin.prototype;
        Target.prototype = new Buffer();
        Target.prototype.constructor = Target;
        Target.prototype.super_class = Origin;
    }
})();

// 某一功能模块
var initProgram = (function () {
    var Program = function () { }
    Program.prototype = {
        name: '程序员',
        tool: 'coding',
        say: function () {
            console.log(
                `工作：${this.name},技能：${this.tool},编程语言：${this.lang.toString()}。`
            );
        }
    }
    var Frontend = function () { }
    var Backend = function () { }

    inherit(Frontend, Program);
    inherit(Backend, Program);

    Frontend.prototype.lang = ['JavaScript', 'CSS', 'HTML'];
    Backend.prototype.lang = ['node', 'Java', 'Golang'];

    return {
        Frontend: Frontend,
        Backend: Backend
    }
})();


var init = function () {
    var frontend = new initProgram.Frontend();
    var backend = new initProgram.Backend();
    backend.say();//工作：程序员,技能：coding,编程语言：node,Java,Golang。
    frontend.say();//工作：程序员,技能：coding,编程语言：JavaScript,CSS,HTML。
}

init();
```

## 构造函数

> 构造函数有着可扩展，**new**（实例化）之后生成互补干扰的特点，是面向对象在es6之前的实现，在命名上区分普通function**首字母大写**。

### 参数问题

```javascript
// 字符串结构
function Car(opt) {
    this.name = opt.name;
    this.color = opt.color;
    this.info = function () {
        console.log(`name: ${this.name},color: ${this.color}`);
    }
}

//优势：以键值对的方式输入参数，可读性强
let benz = new Car({
    name: 'benz',
    color: 'red'
});
benz.info();//name: benz,color: red

console.log('-------------------------------------------');
// 解构传参
function Phone({ }) {
    this.name = name;
    this.color = color;
    this.info = function () {
        console.log(`name: ${this.name},color: ${this.color}`);
    }
}

let duduCar = new Car({
    name: 'benz',
    color: 'red'
});
duduCar.info();//name: benz,color: red
```

## 函数重载（JQuery）

> 建议不要使用，使用es6语法更加舒畅

```javascript
function addMethod(object, name, f) {
    var old = object[name];
    object[name] = function() {
        // f.length为函数定义时的参数个数
        // arguments.length为函数调用时的参数个数
        if (f.length === arguments.length) {
            return f.apply(this, arguments);
        } else if (typeof old === "function") {
            return old.apply(this, arguments);
        }
    };
}

// 不传参数时，返回所有name
function find0() {
    return this.names;
}

// 传一个参数时，返回firstName匹配的name
function find1(firstName) {
    var result = [];
    for (var i = 0; i < this.names.length; i++) {
        if (this.names[i].indexOf(firstName) === 0) {
            result.push(this.names[i]);
        }
    }
    return result;
}

// 传两个参数时，返回firstName和lastName都匹配的name
function find2(firstName, lastName) {
    var result = [];
    for (var i = 0; i < this.names.length; i++) {
        if (this.names[i] === firstName + " " + lastName) {
            result.push(this.names[i]);
        }
    }
    return result;
}

var people = {
    names: ["Dean Edwards", "Alex Russell", "Dean Tom"]
};

addMethod(people, "find", find0);
addMethod(people, "find", find1);
addMethod(people, "find", find2);

console.log(people.find()); // 输出["Dean Edwards", "Alex Russell", "Dean Tom"]
console.log(people.find("Dean")); // 输出["Dean Edwards", "Dean Tom"]
console.log(people.find("Dean", "Edwards")); // 输出["Dean Edwards"]
```

## 函数柯里化

```javascript
var add = function (x) {
    return function (y) {
        return x + y;
    }
}

console.log(add(1)(1));//2

let add1 = add(2);
console.log(add1(3));//5

let add2 = add(4);
console.log(add2(6));//10
```

## 函数式编程

> [wiki函数式编程](https://zh.wikipedia.org/wiki/%E5%87%BD%E6%95%B0%E5%BC%8F%E7%BC%96%E7%A8%8B)
>
> [blog](https://zhuanlan.zhihu.com/p/91040461)
>
> 函数式编程的好处就是代码解构简单，易于查阅，减少使用for loop ，隔离变量，专注于**输入**和**输出**，
>
> 但是过分的使用函数式会导致后期难以维护，代码混乱等问题。
>
> [关于函数式的调侃](https://www.zhihu.com/question/33652103/answer/1323875670)
>
> 函数式三件套
>
> `forEach()` `filter()` `reduce()`
>
> `map()` 该函数与forEach效果类似效率更高，但是会生成新的数组

```javascript
// 遍历，对每一个元素进行操作
// 配合reverse可以反向遍历
let nums = [0, 1, 2];
nums.forEach(item => {
    console.log(item + 1);
})
console.log('-------------------------------------------');

// filter，过滤，保留满足条件的
// 语法：
// var newArray = arr.filter(callback(element[, index[, array]])[, thisArg])

let nums2 = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
let nums3 = nums2.filter(item => {
    return item > 8;
})
console.log(nums3);//[ 9, 10 ]

console.log('-------------------------------------------');
// 保留真值
const arr = [undefined, null, "", 0, false, NaN, 1, 2].filter(Boolean);
console.log(arr);
// [1, 2]

// reduce
// arr.reduce(callback(accumulator, currentValue[, index[, array]])[, initialValue])
// demo 略
```

### 手写forEach

```javascript
        // 手写forEach
        Array.prototype.myForeach = function (fn) {
            let arr = this;
            let len = arr.length;
            let arg2 = arguments[1] || window;
            for (let i = 0; i < len; i++) {
                fn.apply(arg2, [arr[i], i, arr])
            }

        }
```

### 手写filter

> 涉及到新数组就得用深拷贝，可以看我手写系列的深拷贝。

```javascript
Array.prototype.myFilter = function (fn) {
    let arr = this;
    let len = arr.length;
    arg2 = arguments[1] || global;
    let newArr = []
    for (let i = 0; i < len; i++) {
        // fn.apply(arg2, [arr[i], i, arr]) ? newArr.push(arr[i]) : '';
        if(fn.apply(arg2, [arr[i], i, arr])) newArr.push(arr[i])
    }
    return newArr;
}
```

### 手写map

> 原生的map函数操作的时候操作是原数组，此处使用深拷贝重写

```javascript
Array.prototype.myMap = function (fn) {
    let arr = this;
    let len = arr.length;
    arg2 = arguments[1] || global;
    let newArr = [];
    let item;
    for (let i = 0; i < len; i++) {
        item = deepClone(arr[i]);
        newArr.push(fn.apply(arg2, [item, i, arr]));
    }
    return newArr;
}
```

### compose

> 通过管道传值，实现函数合并

```javascript
const compose = (f,g) => (...arg) => f(g(...arg));
let toUpperCase = (x) => x.toUpperCase();
let addPlain = (x) => x + '!';
let shout = compose(toUpperCase, addPlain);
console.log(shout('hello world'));
```

