# 闭包\(closure\)

​	函数和对其周围状态（**lexical environment，词法环境**）的引用捆绑在一起构成**闭包**（**closure**）。也就是说，闭包可以让你从内部函数访问外部函数作用域。在 JavaScript 中，每当函数被创建，就会在函数生成时生成闭包。

​	使用闭包，可以将数据驻留的内存中进行持续的操作，同时可实现一些内容的**预加载**。

​	一般形成方式，父函数的AO被子函数获取，函数的垃圾回收机制只能销毁自身的AO，同时还需要将子函数**return**即**抛出到全局**，父函数的AO就能驻留在内存中。

​	用网友通俗的话讲：**你想从别人家借东西但对方不肯，你只好从他家孩子下手去借。**

缺陷

​	过多的闭包有可能回出现内存泄漏的问题。

## 简要示范

```javascript
function test() {
    var n = 100;
    function add() {
        n++;
        console.log(n);
    }
    function reduce() {
        n--;
        console.log(n);
    }
    return [add, reduce];
}

var arr = test();
arr[0]();//101
arr[1]();//100
arr[1]();//99
arr[1]();//98
arr[1]();//97
arr[1]();//96
arr[1]();//95
arr[1]();//94
arr[1]();//93
```

```javascript
function sunSched() {
    var sunSched = '';
    var operation = {
        setSched: function (thing) {
            sunSched = thing;
        },
        showSched: function () {
            console.log("My schedule on Sunday is " + sunSched);
        }
    }
    return operation;
}

var sunSched = sunSched();
sunSched.setSched('study');
sunSched.showSched();//My schedule on Sunday is study
```

## 较为不常用的实现方式

```javascript
        (function test() {
            var a = 1;
            function add() {
                a++;
                console.log(a);
            }
            window.add = add;
        })();

        add();//2
        add();//3
        add();//4
```

## 预编译

> 1、检查语法
>
> 1.5、预编译（函数执行前的步骤）
>
> 函数声明、整体提升，变量声明提升、赋值不提升。
>
> 2、解释一行执行一行

### 暗示全局变量（imply global variable）

> 只要是不在局部作用用内声明的变量（注：只有使用`var`声明，即过去的缺陷，也就是为什么尽量用`let`声明的原因）会被默认挂载到全局。

```JavaScript
for (var i = 0; i < 10; i++) {
  setTimeout(function(){
    console.log(i);
  })
}
// 输出 10个10
```

### 作用域\[\[scope\]\]

> 执行上下文（execution contexts\)）
>
> 执行上下文栈（Execution context stack，ECS）

### 挂载点

Node.js：`GO === gobal`

浏览器：`GO === window`

### 函数上下文（AO：activation object）

#### ①结论

`AO{`

`1）形参和变量声明`

`2）实参赋值给形参`

`3）函数声明、函数赋值`

`}`

`4）执行、变量赋值……`

`var a = function() {}`属于变量赋值

#### ②示例

```javascript
function test1() {
    var a = b = 1;
    // 正确的书写方式是 var a = 1 , b = 1;
}
test1();
//node.js环境
console.log(a);//undefined 在浏览器环境下是Uncaught ReferenceError: a is not defined
console.log(b);//1
console.log(global.a);//undefined
console.log(global.b);//1
```

**var和let的一个区别**

```javascript
console.log(a);
var a;
```

```javascript
console.log(b);//Cannot access 'b' before initialization
let b;
```

```javascript
function test2(a) {
    console.log(a);//[Function: a]
    var a = 1;
    console.log(a);//1
    function a() { }
    console.log(a);//1
    var b = function test4() { }
    console.log(b);//[Function: test4]
    function d() { }
}

test2(2);

/*
    a: undefined
        2
        function a(){}
    b: undefined
        function b(){}
    d:function d(){}
    开始执行
    a ->1;
*/
```

```javascript
function test3(a, b){
    console.log(b);//[Function: b]
    console.log(a);//1
    c = 0;
    var c;
    a = 5;
    b = 6;
    console.log(a);//5
    console.log(b);//6
    function b(){}
    function d(){}
    console.log(b);//6
}
test3(1);
```

## 全局上下文（GO：gobal object）

#### ①结论

`GO{`

`1）变量声明`

`2）函数声明`

`}`

`3）执行（变量赋值、函数赋值等）`

#### ②示例

```javascript
console.log(a, b);//[Function: a] undefined
function a() { }
var b = function () { }//此处是赋值，所以在预编译时未被提升
console.log(b);//[Function: b]
```

### 练习

```javascript
var b = 3;
console.log(a);//[Function: a]
function a(a) {
    console.log(a);//[Function: a]
    var a = 2;
    console.log(a);//2
    function a() { }
    var b = 5;
    console.log(b);//5
}

a(1);
```

```javascript
var a = 1;
function test(){
    console.log(a);//undefined AO中有a声明，就不向GO中取值
    var a = 2;
    console.log(a);//2
    var a = 3;
    console.log(a);//3
}

test();
var a;
```

```javascript
function test2() {
    console.log(b);//undefined
    console.log(a);//undefined
    if (a) {
        var b = 2;
    }
    c = 3;
    console.log(c);//3
}

var a;
test2();
a = 1;
console.log(a);//1

/*预编译
GO{
    a ：undefined
    test2 : function test2()}{}
    c : 3

}
AO{
    b = undefined;
}*/
function test() {
    return a;
    a = 1;
    function a() {
        var a = 2;
    }
}
// console.log(a);// a is not defined
console.log(test());//[Function: a] return终止函数 使 a = 1 var = 2失效

/* GO{
    a = undefined
}

AO{
    a:undefined
        function a(){}
} */
```

```javascript
var a;
console.log(test2);//[Function: test2]
function test2(e) {
    function e() {}
        arguments[0] = 2;
        console.log(e);//2
        if (a) {//a = undefined
            var b = 3;//未被执行
        }
        var c;
        a = 4//赋值在后
        var a;//声明提升
        console.log(b);//undefined
        f = 5;
        console.log(c);//undefined
        console.log(a);//4
}
console.log("a:"+a);//a:undefined
a =1;
test2(1);
console.log(a);//1
console.log(f);//5


/**
 * 预编译
 * GO{
 *  a:undefined 
 *  test2(){}
 *  f:undefined
 *
 * }
 * AO{
 *  a:undefined 
 *  b:undefined
 *  c:undefined
 *  e:undefined
 *      1
 *          function(){}
 * }
 */
```

```javascript
console.log(test2());//2

function test2() {
    a = 1;
    function a() { }
    var a = 2;
    return a;

}

// console.log(a);//a is not defined
/*
    GO{
        a: undefined
    }
    AO{
        a :undefined
            function a(){}
    }
*/
```

### 父函数无法取得子函数的赋值

```javascript
console.log(test1());//undefined  test1的return 是JavaScript引擎补充上去的undefined

function test1() {
    a = 1;
    console.log(a);//1
    function a() {
        var a = 2;
        return a;
    }
}
```

```javascript
function test3(){
    function a() {
        var a = 2;
        return a;
    }
    var a;
    return a;
}

console.log(test3());//[Function: a]
```

### eval上下文\(略\)

原理

AO和GO

抛出全局的方式：

①：return 子函数

②：window.a = 子函数\(其中的window指的是是全局对象，在node.js中是global\)

## 立即执行函数（IIFE）

IIFE（immediately-invoked function expression）

效果：自动立即执行，执行完之后立即释放

```javascript
/*
(function (){

})();

(function (){//w3c建议，可读性较差

}()); */
(function add() {// 立即执行函数自动忽略函数名
    var a = 1;
    var b = 2;
    console.log(a + b);
}());

// add();//ReferenceError: add is not defined 函数已经被销毁了
```

## 常用定义方式

```javascript
;(function(){})()
;(function(){})()
;(function(){})()
;(function(){})()
```

## 参数

```javascript
(function (a, b) {
    console.log(a + b);
}(1, 2));
```

## 返回值接收

```javascript
var addNum = (function (a, b) {
    return a + b;
}(1, 2));

console.log(addNum);//3
```

## 小结论：

一定是表达式\(expression\)才能被执行符号执行

**表达式：①被\(\)包裹的内容②可以打 ; 的代码段**

执行符号： \(\);

变成表达式后：function name被忽略、name === undefined（见面试题⑤）

```javascript
var test=function (){
    console.log(1);
}();

console.log(test);//undefined 被销毁了

/*
function test3(){
    console.log(1);
}();//SyntaxError: Unexpected token ')'
*/
```

### 函数变成表达式的方式

```javascript
//在函数前添上 + - ！ || && 括号包裹
+ function test3(){
    console.log(1);
}();

1 && function test3(){
    console.log(1);
}();
```

## 单一文件实现模块化开发\(闭包和立即执行函数实践\)

```javascript
var inherit = (function () {
    var Buffer = function () { }
    return function (Target, Origin) {
        Buffer.prototype = Origin.prototype;
        Target.prototype = new Buffer();
        Target.prototype.constructor = Target;
        Target.prototype.super_class = Origin;
    }
})();

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


var frontend = new initProgram.Frontend();
var backend = new initProgram.Backend();
backend.say();//工作：程序员,技能：coding,编程语言：node,Java,Golang。
frontend.say();//工作：程序员,技能：coding,编程语言：JavaScript,CSS,HTML。
```

