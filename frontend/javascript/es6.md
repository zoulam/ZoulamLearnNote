# ES6

[视频地址](https://www.bilibili.com/video/BV1uK411H7on?p=28)

## `let` `const`

### let

> 1、不能重复声明
>
> 2、块作用域 即`{}`内才能使用，当然if else while for内也是块级作用域
>
> 3、不会出现像`var`一样有变量提升，即必须先声明，再使用
>
> 4、不会影响作用域链

```javascript
        {
            let test = 'name';
            function fn(){
                console.log(test);//此处块内没有声明test，便顺着作用域链去找
            }
            fn();
        }
```

#### var的全局声明风险

> var声明的变量，不受块级作用域的影响

![&#x793A;&#x4F8B;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200802144444087.png)

```javascript
<body>
    <div class="container">
        <h2 class="page-header">点击切换颜色</h2>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
    </div>
    <script>
        //获取div元素对象
        let items = document.getElementsByClassName('item');

        //遍历并绑定事件
        for (let i = 0; i < items.length; i++) {
            //若是此处用var声明，function内的i都是=3的
            items[i].onclick = function () {
                //修改当前元素的背景颜色
                // this.style.background = 'pink';
                items[i].style.background = 'pink';
            }
        }

    </script>
</body>
```

### const

> 常量，值不能修改
>
> 声明的引用数据类型的内容是可以修改的，实际引用的是**地址**
>
> 1、声明一定要赋值要赋值
>
> 2、通常大写
>
> 3、无法修改
>
> 4、只能再块级作用域使用
>
> 5、对数组、对象等引用数据类型的**内容**是可修改的

```javascript
        const nums = [1, 2, 3, 4];
        nums.push(5);
```

## 解构赋值

### 数组的解构赋值

```javascript
        // 对象也能解构赋值，此处略
        const F4 = ['小沈阳','刘能','赵四','宋小宝'];
        // 重命名
        let [xiao: xiaoliu, liu, zhao, song] = F4;
        console.log(xiaoliu);
        console.log(liu);
        console.log(zhao);
        console.log(song);
```

#### 多维数组

```javascript
        var [a, [b, c], d] = [1, [2, 3], 4];
        console.log(a, b, c, d);
        var [a, [b, c], d] = [1, [2], 4];
        // 当c未被声明
        console.log(c);//undefined
```

#### 交换值

```javascript
        var [x, y] = [8, 9];
        [x, y] = [y, x];
        console.log(x, y);//9 8
```

#### 接收多个返回值

```javascript
        function show() {
            return ['result1', 'result2', 'result3'];
        }
        var [a, b, c] = show();
        console.log(a, c);
```

#### 快速去除数组值（不建议使用）

```javascript
        var arr = [10, 20, 30, 40, 50];
        var { 0: first, 4: last } = arr;
        console.log(first);//10
        console.log(last == arr[4]);//true
```

### 对象的解构赋值

> 好处，不用使用 `对象.方法`的格式调用方法

```javascript
        const zhao = {
            name: '赵本山',
            age: '不详',
            xiaopin: function(){
                console.log("我可以演小品");
            }
        };

        let {name, age, xiaopin} = zhao;
        console.log(name);
        console.log(age);
        console.log(xiaopin);
        xiaopin();
```

#### 直接声明

```javascript
        var { a, b, c } = {
            a: 7,
            b: 8,
            c: 9
        }
        console.log(a, b, c);
```

#### 传入对象参数

```javascript
        function showInfo({ name, age, sex = '女' }) {
            console.log(`我是名字是${name},我今年${age},我是一个${sex}的`);
        }
        showInfo({
            age: 18,
            name: 'bobo',
            // sex:'男'
        })
```

## 模板字符串

> 能够识别**换行符**，减少字符串拼接时出现的大量引号而导致内容混乱
>
> 内容指出：`${变量/表达式/函数调用}`

```javascript
  `${name}
    <li>${text}</li>
`
```

## 对象和方法的简写

> 先在对象外面声明值，再直接写入。
>
> 方法的简写只能在class和对象内使用。

```javascript
        let name = 'luluxi';
        let change = function(){
            console.log('i am a function!');
        }

        const school = {
            name,
            // 等效: name:name,
            change,
            //
            improve(){
                console.log("just a test");
            }
        }

        console.log(school);
```

## 箭头函数

```javascript
        var fn = () => { }
        // var fn = function(){}

        // 一个参数，语句只有return
        var fn = a => a;
        // var fn = function (a) { return a }
```

### this指向

> this 是静态的. this 始终指向函数**声明时外层作用域下的 this 的值**，无法使用`call()` `apply()`等函数改变

```javascript
        function getName() {
            console.log(this.name);
        }
        let getName2 = () => {// 此时的箭头函数是在全局声明的就是指向this的
            console.log(this.name);
        }

        //设置 window 对象的 name 属性
        window.name = 'lala';
        const DD = {
            name: "luluxi"
        }

        //直接调用
        getName();// lala
        getName2();// lala

        //call 方法调用
        getName.call(DD);//luluxi

        // 箭头函数的this没有被更改
        getName2.call(DD);//lala
```

### 不能使用arguments变量

```javascript
        let fn = ()
            console
        }
        fn(1,2,3);
        // VM40: 2 Uncaught ReferenceError: arguments is not defined
        // at fn(<anonymous>:2:17)
        // at <anonymous>:4:1
```

### 不能作为构造函数实例化对象

> 使用`new`关键字会出错

```javascript
     let Person = (name, age) => {
            this.name = name;
            this.age = age;
        }
        let me = new Person('xiao',30);
        console.log(me);
// VM35:5 Uncaught TypeError: Person is not a constructor
//    at <anonymous>:5:18
// (anonymous) @ VM35:5
```

### 简单实践

> 原因：
>
> 普通函数，默认指向实例化之后的对象，call、apply等都可以改变。
>
> 结论：
>
> 箭头函数适合与 this 无关的回调. 定时器, 数组的方法回调 箭头函数不适合与 this 有关的回调. 事件回调, 对象的方法

```markup
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>箭头函数实践</title>
    <style>
        div {
            width: 200px;
            height: 200px;
            background: #58a;
        }
    </style>
</head>

<body>
    <div id="ad"></div>
    <script>
        //需求-1  点击 div 2s 后颜色变成『粉色』
        //获取元素
        let ad = document.getElementById('ad');
        //绑定事件
        ad.addEventListener("click", function () {//此处不适合箭头函数
            //保存 this 的值， 不然this会指向window
            //默认指向声明时的this值，即ad
            // let _this = this;

            //定时器
            setTimeout(() => {//此时的箭头函数是声明在ad.addEventListener作用域下的
                //修改背景颜色 this
                // console.log(this);
                // _this.style.background = 'pink';
                this.style.background = 'pink';
            }, 2000);
        });

        //需求-2  从数组中返回偶数的元素
        const arr = [1, 6, 9, 10, 100, 25];
        // filter 返回满足条件的值
        // const result = arr.filter(function(item){
        //     if(item % 2 === 0){
        //         return true;
        //     }else{
        //         return false;
        //     }
        // });
        const result = arr.filter(item => item % 2 === 0);

        console.log(result);

        // 箭头函数适合与 this 无关的回调.：定时器, 数组的方法回调
        // 箭头函数不适合与 this 有关的回调： DOM事件回调, 对象的方法
        let obj = {
            name: "zoulam",
            getName: () => {
                console.log(this.name);
            },
            getName2: function () {
                console.log(this.name);
            }
        }

        obj.getName2();
        obj.getName();

    </script>
</body>

</html>
```

[more](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

## 函数参数默认值

> 在过去的没有默认值的情况下都会这样做
>
> `var 参数名 = arguments[0] || 初始值`

```javascript
        //1. 形参初始值 具有默认值的参数, 一般位置要靠后(潜规则)
        function add(a,c=10,b) {
            return a + b + c;
        }
        let result = add(1,2);
        console.log(result);

        //2. 与解构赋值结合
        function connect({host="127.0.0.1", username,password, port}){
            console.log(host)
            console.log(username)
            console.log(password)
            console.log(port)
        }
        connect({
            host: 'localhost',
            username: 'root',
            password: 'root',
            port: 3306
        })

    // es6之前
        function connect(opt){
            console.log(opt.host)
            console.log(opt.username)
            console.log(opt.password)
            console.log(opt.port)
        }
        connect({
            host: 'localhost',
            username: 'root',
            password: 'root',
            port: 3306
        })
```

## rest（写在形参）

> **获取实参**，用来代替`arguments`,
>
> 声明方式`..参数名`
>
> rest是数组类型的内容
>
> arguments是对象类型的内容

```javascript
        // ES6 引入 rest 参数，用于获取函数的实参，用来代替 arguments
        //  ES5 获取实参的方式
         function date(){
             console.log(arguments);
         }
         date('白芷','阿娇','思慧')
         //  rest 参数
         function date(...args){
             console.log(args);//
         }
         date('阿娇','柏芝','思慧')
        //  rest 参数必须要放到参数最后
         function fn(a,b,...args){
             console.log(a);
             console.log(b);
             console.log(args);
         }
         fn(1,2,3,4,5,6)
```

## 扩展运算符（写在实参）

> 作用：
>
> 1、用数组传入一个一个的参数，而不是一个数组
>
> 2、数组克隆、合并
>
> 3、将伪数组转化为真数组，场景：**配合Set实现数组去重**

```javascript
        // 0.传入多个参数
        function test() {
            console.log(arguments);
        }

        let name = ["luluxi", "lala", "momo"];
        test(...name);//此处是传入三个字符串参数，而不是一个数组参数

        // 1. 数组的合并 情圣  误杀  唐探
        const kuaizi = ['王太利', '肖央'];
        const fenghuang = ['曾毅', '玲花'];
        // const zuixuanxiaopingguo =  kuaizi.concat(fenghuang);
        const zuixuanxiaopingguo = [...kuaizi, ...fenghuang];
        console.log(zuixuanxiaopingguo);

        //2. 数组的克隆
        const sanzhihua = ['E', 'G', 'M'];
        const sanyecao = [...sanzhihua];//  ['E','G','M']
        console.log(sanyecao);

        //3. 将伪数组转为真正的数组
        const divs = document.querySelectorAll('div');
        const divArr = [...divs];
        console.log(divArr);// arguments
```

## Symbol

> 1\) Symbol 的值是唯一的，用来解决命名冲突的问题 2\) Symbol 值不能与其他数据进行运算 3\) Symbol 定 义 的 对 象 属 性 不 能 使 用 for…in 循 环 遍 历 ， 但 是 可 以 使 用 Reflect.ownKeys 来获取对象的所有键名

![symbol](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200802160533044.png)

```javascript
        //创建Symbol
        let s = Symbol();
        console.log(s, typeof s);
        let s2 = Symbol('luluxi');
        let s3 = Symbol('luluxi');
    // luluxi只是一个标志，两者是不一样的
        console.log(s2 === s3);//false
        console.log(s2 == s3);//false
        //Symbol.for 创建
        let s4 = Symbol.for('luluxi');
        let s5 = Symbol.for('luluxi');
        console.log(s4 === s5);//true

        //不能与其他数据进行运算
        //    let result = s + 100;
        //    let result = s > 100;
        //    let result = s + s;

        // USONB  you are so niubility
        // u  undefined
        // s  string  symbol
        // o  object
        // n  null number
        // b  boolean
```

### Symbol使用

> 给对象添加属性和方法，表示独一无二的
>
> 场景：当一个对象中有_非常多的属性和方法_，现在需要添加属性和方法，同时又需要**避免重名**，此时使用`Symbol`就能解决这个问题

```javascript
        //向对象中添加方法 up down
        let game = {
            name:'俄罗斯方块',
            up: function(){},
            down: function(){}
        };

        //声明一个对象
         let methods = {
             up: Symbol(),
             down: Symbol()
         };

         game[methods.up] = function(){
             console.log("我可以改变形状");
         }

         game[methods.down] = function(){
             console.log("我可以快速下降!!");
         }

         console.log(game);


        //方法二
        let youxi = {
            name:"狼人杀",
            [Symbol('say')]: function(){
                console.log("我可以发言")
            },
            [Symbol('zibao')]: function(){
                console.log('我可以自爆');
            }
        }

        console.log(youxi)
```

### Symbol内置值

> 自定义命令和function，说白了就是扩展对象的能力

```javascript
        // 实现自定义的类型检测，即自定义instanceof命令
        class Person {
            static [Symbol.hasInstance](param) {
                console.log(param);
                console.log("我被用来检测类型了");
                return false;
            }
        }

        let o = {};

        console.log(o instanceof Person);

        // 将数组设置为不可展开
        const arr = [1,2,3];
        const arr2 = [4,5,6];
        arr2[Symbol.isConcatSpreadable] = false;
        console.log(arr.concat(arr2));//[1, 2, 3, Array(3)]
```

[more](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol#)

### Symbol的实现

[参考](https://juejin.im/post/6844903619544760328#heading-8)

## 迭代器（iterator）

> 原生具备iterator的数据：Array、Arguments、Set、Map、String、TypedArray、NodeList，即可以直接使用`for…of`语法
>
> 其他的数据需要自定义：下面又简单示例
>
> **注：**迭代器的指针初始位置，以数组为例，最开始是指在数组头的前面，而不是数组下标为0的位置，`next`之后才指向下标为0的位置

数组不建议使用`for…in`语法遍历，`for…in`遍历对象语法比较好

![&#x793A;&#x4F8B;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200802164803396.png)

### 自定义迭代器

#### 准备

```javascript
        const xiyou = ['唐僧', '孙悟空', '猪八戒', '沙僧'];
        let iterator = xiyou[Symbol.iterator]();
        console.log(iterator);//见下图
```

![&#x8FED;&#x4EE3;&#x5668;&#x7684;&#x5185;&#x5BB9;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200802165111082.png)

> 工作原理 a\) 创建一个指针对象，指向当前数据结构的起始位置 b\) 第一次调用对象的 next 方法，指针自动指向数据结构的第一个成员 c\) 接下来不断调用 next 方法，指针一直往后移动，直到指向最后一个成员 d\) 每调用 next 方法返回一个包含 **value** 和 **done** 属性的对象 注: 需要自定义遍历数据的时候，要想到迭代器。

示例

```javascript
        const xiyou = ['唐僧', '孙悟空', '猪八戒', '沙僧'];
        let iterator = xiyou[Symbol.iterator]();
        //调用对象的next方法
        console.log(iterator.next());
        console.log(iterator.next());
        console.log(iterator.next());
        console.log(iterator.next());
        console.log(iterator.next());
```

![&#x6548;&#x679C;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200802165517388.png)

#### 实践

> 其实使用`forEach()`也可以完成相应的功能，但是这样不符合面向对象编程，即任何人都可以**直接操作**该对象,用不用就见仁见智了。

```javascript
        //声明一个对象
        const banji = {
            name: "终极一班",
            stus: [
                'xiaoming',
                'xiaoning',
                'xiaotian',
                'knight'
            ],
            [Symbol.iterator]() {
                //索引变量
                let index = 0;
                //
                let _this = this;
                return {
                    next: function () {
                        if (index < _this.stus.length) {
                            const result = { value: _this.stus[index], done: false };
                            //下标自增
                            index++;
                            //返回结果
                            return result;
                        } else {
                            return { value: undefined, done: true };
                        }
                    }
                };
            }
        }

        //遍历这个对象
        for (let v of banji) {
            console.log(v);
        }
        console.log('-------------------------------------------');

        banji.stus.forEach(item => {
            console.log(item);
        })
        console.log('-------------------------------------------');
        for (let out in banji.stus) {
            console.log(out);
        }
```

![&#x6548;&#x679C;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200802171815255.png)

## 生成器（yield）

> 一个特殊的函数，一个解决回调地狱的解决方案
>
> 声明：`function * gen(){}`
>
> 执行：`iterator.next()`

```javascript
        //生成器其实就是一个特殊的函数
        //异步编程  纯回调函数  node fs  ajax mongodb
        // yield是函数代码的分隔符
        function * gen(){
            console.log(111);
            yield '一只没有耳朵';
             console.log(222);
            yield '一只没有尾部';
             console.log(333);
            yield '真奇怪';
             console.log(444);
        }

        let iterator = gen();
        console.log(iterator.next());
        console.log(iterator.next());
        console.log(iterator.next());
        console.log(iterator.next());
    console.log('-------------------------------------------');
        //遍历
        for(let v of gen()){
            console.log(v); // 输出yield的内容
        }
```

![&#x8F93;&#x51FA;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200802172411579.png)

### 参数

```javascript
        function * gen(arg){
            console.log(arg);// AAA
            let one = yield 111;
            console.log(one);// BBB
            let two = yield 222;
            console.log(two);// CCC
            let three = yield 333;
            console.log(three);// DDD
        }

        //执行获取迭代器对象
        let iterator = gen('AAA');// 直接传参
        console.log(iterator.next());
        //next方法可以传入实参
        console.log(iterator.next('BBB'));
        console.log(iterator.next('CCC'));
        console.log(iterator.next('DDD'));
```

### 实践

> 异步编程介绍
>
> JavaScript是单线程语言，异步编程能提高程序执行效率。
>
> 简单理解：在主线程之外开辟一个异步操作（异步代码），实现主线程不会阻塞。
>
> ```text
> Main thread: Task A                   Task B
>     Promise:      |__async operation__|
> ```
>
> [more](https://developer.mozilla.org/zh-CN/docs/learn/JavaScript/%E5%BC%82%E6%AD%A5/%E6%A6%82%E5%BF%B5)
>
> 场景
>
> 文件操作 网络操作\(ajax, request\) 数据库操作 1s 后控制台输出 111 2s后输出 222 3s后输出 333 回调地狱 `setTimeout(() => {` `console.log(111);` `setTimeout(() => {` `console.log(222);` `setTimeout(() => {` `console.log(333);` `}, 3000);` `}, 2000);` `}, 1000);`
>
> 后续维护难

```javascript
        function one(){
            setTimeout(()=>{
                console.log(111);
                iterator.next();
            },1000)
        }

        function two(){
            setTimeout(()=>{
                console.log(222);
                iterator.next();
            },2000)
        }

        function three(){
            setTimeout(()=>{
                console.log(333);
                iterator.next();
            },3000)
        }

        function * gen(){
            yield one();
            yield two();
            yield three();
        }

        //调用生成器函数
        let iterator = gen();
        iterator.next();
```

> 用户信息获取

```javascript
        //模拟获取  用户数据  订单数据  商品数据
        function getUsers(){
            setTimeout(()=>{
                let data = '用户数据';
                //调用 next 方法, 并且将数据传入
                iterator.next(data);
            }, 1000);
        }

        function getOrders(){
            setTimeout(()=>{
                let data = '订单数据';
                iterator.next(data);
            }, 1000)
        }

        function getGoods(){
            setTimeout(()=>{
                let data = '商品数据';
                iterator.next(data);
            }, 1000)
        }

        function * gen(){
            //获取信息的过程是有先后顺序的，前面的信息和后面信息是对应的
            let users = yield getUsers();
            let orders = yield getOrders();
            let goods = yield getGoods();
            console.log(`users:${users},orders:${orders},goods:${goods}`);
        }

        //调用生成器函数
        let iterator = gen();
        iterator.next();
        console.log('reader is pig!');
```

## Promise

> 一个构造函数，有两个参数，三种状态，该对象可以调用两个方法`then(callback,callback)`和`catch()`，且可以**链式调用**，也可以使用`Promise.all()` [more](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)

### 简单使用

```javascript
// fs操作
//1. 引入 fs 模块
const fs = require('fs');

//2. 调用方法读取文件
// fs.readFile('./resources/为学.md', (err, data)=>{
//     //如果失败, 则抛出错误
//     if(err) throw err;
//     //如果没有出错, 则输出内容
//     console.log(data.toString());
// });

//3. 使用 Promise 封装
const p = new Promise(function (resolve, reject) {
    fs.readFile("./resources/为学.md", (err, data) => {
        //判断如果失败
        if (err) {
            reject(err);
        } else {
            //如果成功
            resolve(data);//data是buffer
        }
    });
});

p.then(function (value) {
    console.log(value.toString()); // toString 将buffer转化为String
}, function (reason) {
    console.log("读取失败!!");
});
```

![&#x8F93;&#x51FA;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200802193921798.png)

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>发送 AJAX 请求</title>
</head>

<body>
    <script>
        // 接口地址: https://api.apiopen.top/getJoke
        const p = new Promise((resolve, reject) => {
            //1. 创建对象
            const xhr = new XMLHttpRequest();

            //2. 初始化
            xhr.open("GET", "https://api.apiopen.top/getJoke");

            //3. 发送
            xhr.send();

            //4. 绑定事件, 处理响应结果
            xhr.onreadystatechange = function () {
                //判断
                if (xhr.readyState === 4) {
                    //判断响应状态码 200-299
                    if (xhr.status >= 200 && xhr.status < 300) {
                        //表示成功
                        resolve(xhr.response);
                    } else {
                        //如果失败
                        reject(xhr.status);
                    }
                }
            }
        })

        //指定回调
        p.then(function(value){
            console.log(value);
        }, function(reason){
            console.error(reason);
        });
    </script>
</body>

</html>
```

### then

> 调用 then 方法 then方法的返回结果是 Promise 对象, 对象状态由回调函数的执行结果
>
> 1、如果回调函数中返回的结果是 非 promise 类型的属性, 状态为成功, 返回值为对象的成功的值

```javascript
        //创建 promise 对象
        const p = new Promise((resolve, reject)=>{
            setTimeout(()=>{
                resolve('用户数据');
                // reject('出错啦');
            }, 1000)
        });

        const result = p.then(value => {
            console.log(value);
            //1. 非 promise 类型的属性，不写默认是undefined也是resolve状态
             return 'iloveyou';
        }, reason=>{
            console.warn(reason);
        });

    console.log(result);
```

![&#x8F93;&#x51FA;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200802195659506.png)

![&#x4E0D;&#x5199;return](C:/Users/zoulam/AppData/Roaming/Typora/typora-user-images/image-20200802195747288.png)

```javascript
        //创建 promise 对象
        const p = new Promise((resolve, reject) => {
            setTimeout(() => {
                resolve('用户数据');
                // reject('出错啦');
            }, 1000)
        });

        const result = p.then(value => {
            console.log(value);
            //2. 是 promise 对象
            return new Promise((resolve, reject) => {
                // resolve('ok');// [[PromiseStatus]]:"fulfilled"   [[PromiseValue]]:"ok"
                reject('error');// value
            });
        }, reason => {
            console.warn(reason);
        });

        console.log(result);
```

![&#x8F93;&#x51FA;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200802200009742.png)

```javascript
        //创建 promise 对象
        const p = new Promise((resolve, reject) => {
            setTimeout(() => {
                resolve('用户数据');
                // reject('出错啦');
            }, 1000)
        });

        const result = p.then(value => {
            console.log(value);
            //3. 抛出错误
            // throw new Error('出错啦!');
            throw '出错啦!';
        }, reason => {
            console.warn(reason);
        });

        console.log(result);
```

![&#x8F93;&#x51FA;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200802200359750.png)

#### 链式调用

```javascript
        p.then(value=>{

        }).then(value=>{

        });
```

### 实践

```javascript
const fs = require("fs");

// fs.readFile('./resources/为学.md', (err, data1)=>{
//     fs.readFile('./resources/插秧诗.md', (err, data2)=>{
//         fs.readFile('./resources/观书有感.md', (err, data3)=>{
//             let result = data1 + '\r\n' +data2  +'\r\n'+ data3;
//             console.log(result);
//         });
//     });
// });

//使用 promise 实现
const p = new Promise((resolve, reject) => {
    fs.readFile("./resources/为学.md", (err, data) => {
        resolve(data);
    });
});

p.then(value => {
    return new Promise((resolve, reject) => {
        fs.readFile("./resources/插秧诗.md", (err, data) => {
            resolve([value, data]);
        });
    });


}).then(value => {
    return new Promise((resolve, reject) => {
        fs.readFile("./resources/观书有感.md", (err, data) => {
            //压入
            value.push(data);
            resolve(value);
        });
    })

// 用\r\n隔开文件内容
}).then(value => {
    console.log(value.join('\r\n'));
});
```

### catch

```javascript
        const p = new Promise((resolve, reject) => {
            setTimeout(() => {
                reject("出错啦!");
            }, 1000)
        });

        p.catch((reason) => {
            console.warn(reason);
        });
```

### `Promise.all()`

```javascript
var p1 = new Promise(resolve => {
    setTimeout(() => {
        resolve(1);
    }, 1000);
});
var p2 = new Promise(resolve => {
    setTimeout(() => {
        resolve(2);
    }, 2000);
});
var p3 = new Promise(resolve => {
    setTimeout(() => {
        resolve(3);
    }, 500);
});

Promise.all([p1, p2, p3]).then(values =>  console.log(values) );//是用户设定的顺序而不是完成的顺序
```

## Set、Map

### Set使用

> Set具有无序，不重复的特点

| 属性/方法 | 效果 |
| :--- | :--- |
| `size` | 元素个数 |
| `add()` | 插入元素 |
| `delete()` | 删除元素 |
| `has()` | 判断元素是否存在 |
| `clear()` | 清空内容 |
|  |  |
| 被`for…of`遍历的方法返回值:`keys()`   `values()`  `entries()` |  |

```javascript
        var sb = new Set([1, 2, 3, 5, 3, 3, '3']);
        console.log(sb.keys());//[Set Iterator] { 1, 2, 3, 5, '3' }
        console.log(sb.values());//[Set Iterator] { 1, 2, 3, 5, '3' }
        console.log(sb.entries());//[Set Entries] { [ 1, 1 ], [ 2, 2 ], [ 3, 3 ], [ 5, 5 ], [ '3', '3' ] }
    //声明一个 set
        let s = new Set();
        let s2 = new Set(['大事儿','小事儿','好事儿','坏事儿','小事儿']);

        //元素个数
        console.log(s2.size);
        //添加新的元素
        s2.add('喜事儿');
        //删除元素
        s2.delete('坏事儿');
        //检测
        console.log(s2.has('糟心事'));
        //清空
        s2.clear();
        console.log(s2);

        for(let v of s2){
            console.log(v);
        }
```

### 常用操作

```javascript
        //用途：数组去重
        let set = new Set([2, 3, 4, 4, 2, 3, 3, 4, 4, 5, 5, 6, 6, 7, 5, 32, 3, 4, 5]);
        console.log(set);//Set(7) {2, 3, 4, 5, 6, …}
        let arr = [...set];
        console.log(Object.prototype.toString.call(arr));//[object Array]

        // 习惯写法
        let numbers = [1, 1, 1, 1, 2, 2, 2, 2, 3, 3, 3, 3, 3];
        let newNums = [...new Set(numbers)];

        //2. 交集：共有
        let arr2 = [4,5,6,5,6];
        let result = [...new Set(arr)].filter(item => {
            let s2 = new Set(arr2);// 4 5 6
            if(s2.has(item)){
                return true;
            }else{
                return false;
            }
        });
        let result = [...new Set(arr)].filter(item => new Set(arr2).has(item));
        console.log(result);

        //3. 并集：合并
        let union = [...new Set([...arr, ...arr2])];
        console.log(union);

        //4. 差集：交集取反,此处是剔除arr中arr2包含的内容
        let diff = [...new Set(arr)].filter(item => !(new Set(arr2).has(item)));
        console.log(diff);
```

### Map使用

> 键值对存储，高效存取

| 方法/属性 | 效果 |
| :--- | :--- |
| `size` | 元素（键值对）个数 |
| `set()` | 添加元素 |
| `delete(key)` | 删除元素 |
| `get(key)` | 获取元素 |
| `clear()` | 清空Map |

```javascript
        //声明 Map
        let m = new Map();

        //添加元素
        m.set('name','luluxi');
        m.set('change', function(){
            console.log("我们可以改变你!!");
        });
        let key = {
            school : 'ATGUIGU'
        };
        m.set(key, ['北京','上海','深圳']);

        //size
         console.log(m.size);

        //删除
         m.delete('name');

        //获取
         console.log(m.get('change'));
         console.log(m.get(key));

        //清空
         m.clear();

        //遍历
        for(let v of m){
            console.log(v);
        }

        // console.log(m);
```

## 面向对象

### 简单认识

```javascript
        //手机
        function Phone(brand, price){
            this.brand = brand;
            this.price = price;
        }

        //添加方法
        Phone.prototype.call = function(){
            console.log("我可以打电话!!");
        }

        //实例化对象
        let Huawei = new Phone('华为', 5999);
        Huawei.call();
        console.log(Huawei);

        //class
        class Shouji{
            //构造方法 名字不能修改
            //new 的过程中自动执行
            constructor(brand, price){
                this.brand = brand;
                this.price = price;
            }
         // call:function(){}//是错误的，class内不支持这种写法
            //方法必须使用该语法, 不能使用 ES5 的对象完整形式
            call(){
                console.log("我可以打电话!!");
            }
        }

        let onePlus = new Shouji("1+", 1999);

        console.log(onePlus);
```

### static

```javascript
function Phone(){}


        Phone.name = '手机';
        Phone.change = function(){
            console.log("我可以改变世界");
        }
        Phone.prototype.size = '5.5inch';

        let moto = new Phone();

        // function.var的方式是属于函数的，实例化的对象是无法调用的
        // 使用 .xx的形式添加的属性和方法是属于phone而不能被实例化对象 nokia取得（静态属性）

        console.log(moto.name);// undefined
        console.log(moto.change);// undefined

        console.log(moto.size);// 5.5inch

        console.log('-------------------------------------------');
        class shouJi {
            //静态属性
            static name = '手机';
            static change() {
                console.log("我可以改变世界");
            }
        }

        let nokia = new shouJi();
        console.log(nokia.name);// undefined
        console.log(shouJi.name);// 手机
```

### 继承\(extends\)

#### **old**

```javascript
        //手机
        function Phone(brand, price){
            this.brand = brand;
            this.price = price;
        }

        Phone.prototype.call = function(){
            console.log("我可以打电话");
        }

        //智能手机
        function SmartPhone(brand, price, color, size){
            // this 指向SmartPhone的实例化对象
            Phone.call(this, brand, price);
            this.color = color;
            this.size = size;
        }

        //设置子级构造函数的原型
        SmartPhone.prototype = new Phone;
    //校正构造器的指向
        SmartPhone.prototype.constructor = SmartPhone;

        //声明子类的方法
        SmartPhone.prototype.photo = function(){
            console.log("我可以拍照")
        }

        SmartPhone.prototype.playGame = function(){
            console.log("我可以玩游戏");
        }

        const chuizi = new SmartPhone('锤子',2499,'黑色','5.5inch');

        console.log(chuizi);
```

#### **new** `extends` `super()`

> `spuer()`只能出现在**constructor**方法内
>
> 方法重写是指子类对父类同名方法进行覆盖

```javascript
    class Phone{
            //构造方法
            constructor(brand, price){
                this.brand = brand;
                this.price = price;
            }
            //父类的成员属性
            call(){
                console.log("我可以打电话!!");
            }
        }

        class SmartPhone extends Phone {
            //构造方法
            constructor(brand, price, color, size){
                super(brand, price);  // Phone.call(this, brand, price)
                this.color = color;
                this.size = size;
            }

            photo(){
                console.log("拍照");
            }

            playGame(){
                console.log("玩游戏");
            }
            //方法重写 ，子类覆盖父类
            call(){
                console.log('我可以进行视频通话');
            }
        }

        const xiaomi = new SmartPhone('小米',799,'黑色','4.7inch');
        // console.log(xiaomi);
        xiaomi.call();
        xiaomi.photo();
        xiaomi.playGame();
```

### geter setter

```javascript
        // get 和 set
        // 没有constructor 也是合法的
        class Phone {
            get price() {
                // 使用场景，求总数，平均数……数据动态变化的
                console.log("价格属性被读取了");
                return 'iloveyou';
            }
            set price(newVal) {
                // 场景控制和判断,比如合法性判断对传入值限制
                console.log('价格属性被修改了');
            }
        }

        //实例化对象
        let s = new Phone();

        console.log(s.price);
        s.price = 'free';
```

## 数值扩展

```javascript
        //0. Number.EPSILON 是 JavaScript 表示的最小精度
        //EPSILON 属性的值接近于 2.2204460492503130808472633361816E-16
        function equal(a, b) {
            if (Math.abs(a - b) < Number.EPSILON) {
                return true;
            } else {
                return false;
            }
        }
        console.log(0.1 + 0.2 === 0.3);// flase
        console.log(equal(0.1 + 0.2, 0.3))// true
        console.log('-------------------------------------------');

        //1. 二进制和八进制（box）[2 8 16]
        let b = 0b1010;
        let o = 0o777;
        let d = 100;
        let x = 0xff;
        console.log(x);//255
        console.log('-------------------------------------------');

        //2. Number.isFinite  检测一个数值是否为有限数
        console.log(Number.isFinite(100));//true
        console.log(Number.isFinite(100 / 0));// false
        console.log(Number.isFinite(Infinity));//false
        console.log('-------------------------------------------');

        //3. Number.isNaN 检测一个数值是否为 NaN
        console.log(Number.isNaN(123));//false
        console.log('-------------------------------------------');

        //4. Number.parseInt Number.parseFloat字符串转整数
        console.log(Number.parseInt('5211314love'));// 5211314
        console.log(Number.parseFloat('3.1415926神奇'));// 3.1415926
        console.log(`精度保证:${parseFloat((0.2 + 0.1).toFixed(10))}`);//精度保证:0.3
        //parseFloat 除了可以将字符串解析为浮点数，还有一个功能就是把类似2.3002000这种小数的尾部的0都去掉。
        //toFixed(10)只保留10位

        console.log('-------------------------------------------');
        //5. Number.isInteger 判断一个数是否为整数
        console.log(Number.isInteger(5));//true
        console.log(Number.isInteger(2.5));//false
        console.log(Number.isInteger(2.0));//true

        console.log('-------------------------------------------');
        //6. Math.trunc 将数字的小数部分抹掉
        console.log(Math.trunc(3.5));//3
        console.log(Math.trunc(3.4));//3
        // 向下取整
        console.log(Math.floor(-1.2));//-2
        console.log(Math.floor(-1.9));//-2
        console.log(Math.floor(2.9));//2
        console.log(Math.floor(2.1));//2
        // 四舍五入
        console.log(Math.round(2.4));//2
        console.log(Math.round(2.5));//3
        console.log(Math.round(-2.4));//-2
        console.log(Math.round(-2.5));//-2
        console.log(Math.round(-2.6));//-3

        console.log('-------------------------------------------');
        //7. Math.sign 判断一个数到底为正数 负数 还是零
        console.log(Math.sign(100));//1
        console.log(Math.sign(0));//0
        console.log(Math.sign(-20000));//-1
```

## 对象扩展

```javascript
        //1. Object.is 判断两个值是否完全相等
        console.log(Object.is(120, 120));// 与===类似
        console.log(Object.is(NaN, NaN));// true
        console.log(NaN === NaN);// false

        //2. Object.assign 对象的合并(实践场景：配置合并) ，后面对象的value会覆盖同名key的value
        // 实际场景若是出现配置错误，则合并配置对象
        const config1 = {
            host: 'localhost',
            port: 3306,
            name: 'root',
            pass: 'root',
            test: 'test'
        };
        const config2 = {
            host: 'http://luluxi.com',
            port: 33060,
            name: 'atguigu.com',
            pass: 'iloveyou',
            test2: 'test2'
        }
        console.log(Object.assign(config1, config2));

        //3. Object.setPrototypeOf 设置原型对象  Object.getPrototypeof
        const school = {
            name: 'lului'
        }
        const cities = {
            xiaoqu: ['北京', '上海', '深圳']
        }

        Object.setPrototypeOf(school, cities);//效率比较差，不如直接创建直接设置
        console.log(school);
        console.log(Object.getPrototypeOf(school));//输出如下图
```

![&#x8F93;&#x51FA;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200802232537434.png)

![&#x5B98;&#x65B9;&#x5EFA;&#x8BAE;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200802232903851.png)

## 模块化

### 模拟模块化

> 立即执行**函数+闭包**实现在单文件内进行模块化开发

### 简要介绍

> 好处：
>
> 防止命名冲突
>
> 实现代复用
>
> 方便维护
>
> common.js
>
> 支持NodeJS和Browserify
>
> AMD.js
>
> requireJS
>
> CMD.js
>
> seaJS 国人阿里员工玉伯开发

### es6模块化语法

> 使用`export`导出，对外暴露接口（英文意思是：输出，出口）
>
> 使用`import`引入其他模块的功能
>
> 使用`default`导出默认接口
>
> 使用`as`重命名，避免不同模块之间存在同名内容
>
> `import * as ${moduleName} from "${filePath}"`
>
> `*`是通配符，即导入指定模块的全部内容
>
> 在HTML页面中引入需要加上`type="module"` 示例：`<script src="./src/js/app.js" type="module"></script>`

#### export细节

```javascript
//m1.js
//分别暴露
export let school = 'luluxi';

export function teach() {
    console.log("我们可以教给你开发技能");
}


// m2.js
//统一暴露
let school = 'luluxi';

function findJob(){
    console.log("我们可以帮助你找工作!!");
}

//
export {school, findJob};


//m3.js
//默认暴露
export default {
    school: 'LULUXI,
    change: function(){
        console.log("我们可以改变你!!");
    }
}


// m4.js
// 默认暴露
function formatText (){}
export default formatText;
```

#### import细节

```javascript
         // 2. 解构赋值形式
         import {school, teach} from "./src/js/m1.js";
         import {school as zoulam, findJob} from "./src/js/m2.js";
       // 非简写导入default
         import {default as m3} from "./src/js/m3.js"
        // 3. 简便形式  针对默认暴露，不用使用as就可以直接重命名
         import m3 from "./src/js/m3.js";
```

### 打包

> 直接引入HTML文件会使得文件快速膨胀，所以需要将内容汇总到一个`app.js`文件。 为了实现兼容性则需要打包使用，原理是转化成**commonJS**
>
> 1. 安装工具 `npm i babel-cli babel-preset-env browserify(webpack) -D`
>
>    bash : npm i babel-cli babel-preset-env browserify -D
>
>    i 是install 的缩写 -D 是开发依赖 dependencies
>
>    babel-cli babel命令行工具
>
>    babel-preset-env 将es新语法转化为浏览器兼容的es5，同时帮助填入配置参数
>
>    browserify 轻量打包工具无需像webpack那样需要配置
>
> 2. 编译 `npx babel src/js -d dist/js --presets=babel-preset-env`
>
>    全局安装直接使用babel 局部安装则加上npx 原始目录：src/js 编译后的存储目录：dist/js
>
>    传参：--presets=babel-preset-env
>
> 3. 打包 `npx browserify dist/js/app.js -o dist/bundle.js`
>
>    源文件：dist/js/app.js 打包后（编译后）文件dist/bundle.js
>
>    转化后的require依然无法被识别，还需要打包

