---
description: ES5原型继承、深拷贝、Promise实现
---

# 常用代码段

## ES5原型继承

```javascript
let inherit = (function () {
    let Buffer = function () { };
    return function (Target, Origin) {
        Buffer.prototype = Origin.prototype;
        Target.prototype = new Buffer();
        Target.prototype.constructor = Target;
        Target.prototype.super_class = Origin;
    }
})();
```

### 使用call实现

> 简单好用，缺陷 `"use strict"` 会出错

```JavaScript
function Father() {
    this.name = "luluxi"
}

function Children() {
    Father.call(this);
}

let father = new Father();
let children = new Children(father)

console.log(children.name);//luluxi
children.name = 'fuck'
console.log(father.name);// luluxi
console.log(children.name);// fuck
```



## 手写bind、call、apply

```javascript
function show(...args) {
    console.log(args);
    console.log(this.name);
}

Function.prototype.rCall = function (ctx, ...args) {
    let fn = Symbol(1);
    // 将方法挂载ctx上,this指向的rCall函数的调用者
    ctx[fn] = this;
    // 执行挂载的方法
    ctx[fn](...args);
    // 执行完成之后删除原方法
    delete ctx[fn];
}

show.rCall({ name: 'rCall' }, 'args1', 'args2')

Function.prototype.rApply = function (ctx, arr = []) {
    let fn = Symbol(1);
    if (arr && !(arr instanceof Array)) {
        throw ('arguments[1] not a Array');
    }
    ctx[fn] = this;
    ctx[fn](...arr);
    delete ctx[fn];
}
show.rApply({ name: 'rApply' }, ['args1', 'args2'])

Function.prototype.rBind = function (ctx, ...args1) {
    let fn = Symbol(1);
    return (...args2) => {
        ctx[fn] = this;
        ctx[fn](...args1.concat(args2))
        delete ctx[fn];
    }
}

let obind = show.rBind({ name: 'rBind' }, 'args1', 'args2')
obind('args3')
```

## 深拷贝

> 每个版本都有缺陷，看情况使用哪个版本

```

```



## 防抖、节流函数

> 防抖：
>
>  1、事件触发n秒后才执行的回调，延迟执行
>
>  2、在n秒内再次触发事件，重新计时
>
>  3、目的：减少事件触发次数，避免出现抖动
>
>  4、场景：ajax提交：首次提交立即执行，从第二次开始使用防抖
>
>  输入验证
>
>  下拉刷新
>
>  常见的提示：您的操作过于频繁请稍后再试

```javascript
    // time防抖是时间单位ms
    // fn 执行的回调函数
    // triggleNow 当前触发器,首次进入是否防抖
 function debounce(fn, time, triggleNow) {
            let t = null;
            let debounced = function () {
                let ctx = this;
                let args = arguments;
                if (t) {
                    clearTimeout(t);
                }
                // 第二次开始防抖
                if (triggleNow) {
                    let exec = !t;

                    // n秒内进入清除t的id
                    t = setTimeout(() => {
                        t = null;
                    }, time)

                    // 首次进入时 t = null
                    // exec 为真 所以会直接执行
                    if (exec) {
                        fn.apply(ctx, args);
                        // triggleNow = false;
                    }
                } // 首次开始防抖
                else {
                    t = setTimeout(() => {
                        fn.apply(ctx, args);
                    }, time)
                }
            }
            // 清除防抖
            debounced.remove = function () {
                clearTimeout(t);
                t = null;
            }
            return debounced
        }
```

简单版

```javascript
        function anti_shake(func, time, start) {
            let t = null;
            let times = 1;
            return function () {
                let context = this
                let args = arguments
                if (t) clearTimeout(t)
                if (start < times) {
                    t = setTimeout(() => {
                        func.apply(context, args);
                    }, time)
                } else {
                    func.apply(context, args);
                    times++;
                }
            }
        }
```

节流

> 在n秒之内不管触发多少次，都只执行一次
>
>  输入验证
>
>  悬停出现子菜单
>
>  搜索框中的ajax请求
>
>  记录用户行为，如：用户在页面的某处停留了多久

```javascript
    function throttle(func, time) {
            let t = null
            return function () {
                let context = this
                let args = arguments
                if (!t) {
                    // 能执行才清空
                    t = setTimeout(() => {
                        t = null
                        func.apply(context, args)
                    }, time)
                }
            }
        }
```

方式二

```javascript
 function throttle(fn, delay) {
            let t = null;
            let begin = Date.now();
            return function () {
                let _self = this;
                let args = arguments;
                let cur = Date.now();

                clearTimeout(t);
                if (cur - begin > delay) {
                    fn.apply(_self, args);
                    begin = cur;
                } else {
                    t = setTimeout(() => {
                        fn.apply(_self, args);
                    }, delay)

                }
            }
        }
```

## 懒加载



## 手写Promise

### all

```javascript
Promise.myAll = function (promises) {
    // 适合彼此依赖的情况
    // 还可以进行优化，传入数组中的元素中有不是Promise对象的可以封装成对象
    for (let i = 0; i < promises.length; i++) {
        if (!(promises[i] instanceof Promise)) {
            new Promise((res, rej) => {
                res(promises[i]);
            })
        }
    }
    return new Promise((resolve, reject) => {
        let haveError = false;
        let index = 0;
        let runCount = 0;
        const values = [];
        for (const promise of promises) {
            // 这里需要存储执行的promises的index，
            // 异步执行会导致执行时的index == promises.length
            let curIndex = index;
            promise.then(value => {
                if (haveError) return;
                values[curIndex] = value;
                runCount++;
                if (runCount === promises.length) {
                    resolve(values);
                }
            }, err => {
                if (haveError) return;
                haveError = true;
                reject(err)
            })
            index++;
        }
        if (index === 0) {
            resolve([]);
            return;
        }
    });
}




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

var p4 = new Promise((resolve, reject) => {
    setTimeout(() => {
        reject('我是错误4');
    }, 500);
});

// 与执行顺序无关，用户怎么写入就按什么顺序打印
Promise.all([p1, p2, p3]).then(values => console.log(values));
Promise.myAll([p1, p2, p3]).then(values => console.log(values));
```

### race

```JavaScript
Promise.myRace = function (promises) {
    return new Promise((resolve, reject) => {
        // 添加状态判断，只要执行过了就关闭状态
        let status = false;
        for (const promise of promises) {
            if (status) return;
            promise.then(
                (value) => {
                    if (status) return;
                    status = true;
                    resolve(value);
                },
                (err) => {
                    if (status) return;
                    status = true;
                    reject(err);
                });
        }
    });
}

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

// 谁快执行谁
Promise.race([p1, p2, p3]).then(values => console.log(values));
Promise.myRace([p1, p2, p3]).then(values => console.log(values));
```

### allSettled

```javascript
Promise.myAllSettled = function (promises) {
    // 适合互不关联的情况，就算reject也不会终止执行

    return new Promise((resolve, reject) => {
        let elCount = 0;
        const result = [];
        function addElementToResult(index, elem) {
            result[index] = elem;
            elCount++;
            if (elCount === result.length) {
                resolve(result);
            }
        }

        let index = 0;
        // 跟all一样处理promises，但是不会执行reject(),只执行封装之后的resolve
        for (const promise of promises) {
            const curIndex = index;
            promise.then(
                (value) => addElementToResult(
                    curIndex, {
                    status: 'fulfilled',
                    value
                }),
                (reason) => addElementToResult(
                    curIndex, {
                    status: 'rejected',
                    reason
                }));
            index++;
        }


        if (index === 0) {
            resolve([]);
            return;
        }
    });
}

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

var p4 = new Promise((resolve, reject) => {
    setTimeout(() => {
        reject('我是错误4');
    }, 500);
});

Promise.allSettled([p1, p2, p4]).then(values => console.log(values));
Promise.myAllSettled([p1, p2, p4]).then(values => console.log(values));
```

## 手写双向绑定

### Object.defineProperty(Vue2.0)

```javascript
// 观察者（包含发布订阅），此处应该称之为观察者
// 给发生数据变化的内容添加观察者
class Dep {
    constructor() {
        // 存放观察者（Watcher），数据发生变化就通知他
        this.subs = [];
    }
    // 订阅
    addSub(watcher) {
        this.subs.push(watcher);
    }
    // 发布
    notify() {// 通知
        this.subs.forEach(watcher => {
            return watcher.update();
        })
    }
}


// 监视user的name值,只要值发生变化就执行第三个参数的回调函数
// vm.$watch(vm, 'user.name',(newVal)=> {})
class Watcher {
    constructor(vm, expr, cb) {
        this.vm = vm;
        this.expr = expr;
        this.cb = cb;
        this.oldValue = this.get()
    }
    // get(vm, expr) {
    //     let value = CompilerUtil.getValue(vm, expr)
    //     return value
    // }

    get() {
        // 将watcher放在 发布订阅器的target上
        Dep.target = this; //将watcher放在Dep上
        let value = CompilerUtil.getValue(this.vm, this.expr);
        Dep.target = null;// 此处不取消，任何值都会添加到watcher上
        return value;
    }
    update() {
        // 比对新值和旧值，不同才执行回调函数
        let newVal = CompilerUtil.getValue(this.vm, this.expr);
        if (newVal !== this.oldValue) {
            this.cb(newVal);
        }
    }
}


// 实现数据劫持
class Observer {
    constructor(data) {
        this.observer(data);
    }

    // 观察者，让对象封装成能直接被defineProperty处理的值
    observer(data) {
        // 只观察对象
        if (data && typeof data == 'object') {
            for (const key in data) {
                this.defineReactive(data, key, data[key]);
            }
        }
    }
    defineReactive(obj, key, value) {
        this.observer(value);
        // 此处实例化dep
        let dep = new Dep();
        Object.defineProperty(obj, key, {
            get() {
                // 创建watcher时，会取得对应的内容，并且将watcher放在全局上
                // get方法内写入订阅
                Dep.target && dep.addSub(Dep.target);
                // console.log('run getter');
                return value;
            },
            set: (newval) => {
                if (newval !== value) {
                    // console.log('run setter');
                    this.observer(newval);
                    value = newval;
                    // set方法内写入发布（通知）
                    dep.notify();
                }
            }
        })
    }
}


// 编译功能，包含文本节点和元素节点的编译
class Compiler {
    // vm 是Vue的对象实例， el是元素节点
    constructor(el, vm) {
        this.el = this.isElementNode(el) ? el : document.querySelector(el);
        this.vm = vm;
        let fragment = this.nodeFragment(this.el);
        this.compiler(fragment)
        this.el.appendChild(fragment);
    }
    isDirective(key) {
        return key.startsWith('v-');
    }
    compilerElement(node) {
        let attributes = node.attributes;
        [...attributes].forEach(attr => {
            let { name, value: expr } = attr;
            // 解析name是不是vue指令
            if (this.isDirective(name)) {
                // v-model v-on:click
                let [, directive] = name.split('-');
                // model on:click
                let [directiveName, eventName] = directive.split(':');
                // directiveName: on    eventName: click

                // node 编译的节点，
                // expr如:       "user.name"
                // this.vm Vue对象实例
                CompilerUtil[directiveName](node, expr, this.vm, eventName);
            }
        })
    }
    compilerText(node) {
        let content = node.textContent;
        if (/\{\{(.+?)\}\}/.test(content)) {
            // content 是 {{key:value}}
            CompilerUtil['text'](node, content, this.vm)
        }
    }
    compiler(node) {
        // 只编译第一层
        let childNodes = node.childNodes;
        [...childNodes].forEach(child => {
            if (this.isElementNode(child)) {
                // 元素节点
                this.compilerElement(child)
                // 继续深层编译
                this.compiler(child)
            } else {
                // 文本节点
                this.compilerText(child)
            }
        })
    }
    nodeFragment(node) {
        let fragment = document.createDocumentFragment(node);
        let firstChild;
        while (firstChild = node.firstChild) {
            // 将原本页面内的节点移动到文档碎片中
            fragment.appendChild(firstChild)
        }
        return fragment;
    }
    isElementNode(node) {
        return node.nodeType === 1;
    }
}

// 将指令功能存放在对象中
CompilerUtil = {
    getValue(vm, expr) {
        // let arr = expr.split('.');
        // if (arr.length === 1) {
        //     return vm.$data[expr];
        // }

        // 从Vue实例的data对象中解析 . 语法 获取值
        let ans = expr.split('.').reduce((data, curr) => {
            return data[curr];
            // data[curr]等价于user['name']
            // 从Vue的实例中的data中取值
        }, vm.$data)
        return ans;
    },
    setValue(vm, expr, value) {
        expr.split('.').reduce((data, curr, index, arr) => {
            if (index == arr.length - 1) {
                return data[curr] = value; // 最后一次时给value赋值
            }
            return data[curr];
        }, vm.$data)
    },
    // node 编译的节点，
    // expr如:       "user.name"
    // this.vm Vue对象实例
    model(node, expr, vm) {
        // vm[expr] = vm.$data['user.name']
        let fn = this.update['modelUpdater']
        new Watcher(vm, expr, (newValue) => {
            fn(node, newValue);
        });

        node.addEventListener('input', (e) => {
            // 获取用户输入值
            let value = e.target.value
            this.setValue(vm, expr, value);
        })
        let value = this.getValue(vm, expr)

        fn(node, value);
    },
    html(node, expr, vm) {
        let fn = this.update['htmlUpdater']
        new Watcher(vm, expr, (newValue) => {
            fn(node, newValue);
        });
        let value = this.getValue(vm, expr)
        fn(node, value);
    },
    getContentValue(vm, expr) {
        return expr.replace(/\{\{(.+?)\}\}/g, (...args) => {
            return this.getValue(vm, args[1]);
        })
    },
    // text是{{user.name}}
    text(node, expr, vm) {
        // 取出 user.name user.age
        let content = expr.replace(/\{\{(.+?)\}\}/g, (...args) => {

            new Watcher(vm, args[1], () => {
                let changeVal = this.getContentValue(vm, expr);
                fn(node, changeVal)
            })

            return this.getValue(vm, args[1])
        })
        let fn = this.update['textUpdater'];
        fn(node, content)
    },
    on(node, expr, vm, eventName) {
        node.addEventListener(eventName, (e) => {
            // console.log(vm, expr);
            // vm 是vue实例，epxr是"change"
            vm[expr].call(vm, e);
        })
    },
    update: {
        htmlUpdater(node, value) {
            node.innerHTML = value;
        },
        modelUpdater(node, value) {
            node.value = value;
        },
        textUpdater(node, value) {
            node.textContent = value;
        }
    }
}


class Vue {
    constructor(options) {
        this.$el = options.el;
        this.$data = options.data;
        let computed = options.computed;
        let methods = options.methods;
        if (this.$el) {
            // 1、实现数据劫持
            new Observer(this.$data)
            // console.log(this.$data);

            for (let key in computed) {
                Object.defineProperty(this.$data, key, {
                    get: () => {
                        return computed[key].call(this);
                    }
                })
            }

            for (let key in methods) {
                Object.defineProperty(this, key, {
                    get: () => {
                        return methods[key];
                    }
                })
            }

            // 2、vm. 实现等价于 vm.$data
            this.proxyVm(this.$data)
            // 3、元素存在就开始编译
            new Compiler(this.$el, this)
        }
    }

    proxyVm(data) {// 遍历vm实例的时候 从data上取值 即 vm.$data.user.name == vm.user.name
        for (let key in data) {
            Object.defineProperty(this, key, {
                get() {
                    return data[key];
                },
                set: (newVal) => { // 设置代理方法
                    data[key] = newVal;
                }
            })
        }
    }
}
```

### 测试代码

```javascript
<!DOCTYPE html>
<html lang="zh">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <div id="app">
        <input type="text" v-model="user.name" />
        <div>{{user.name}}</div>
        <div>{{user.age}}</div>
        <!-- 如果数据不变化 视图就不刷新 -->
        {{getNewName}}
        <!-- 在vue内部会匹配 {{}} -->
        <ul>
            <li>1</li>
            <li>2</li>
        </ul>
        <button v-on:click="change">更新年龄</button>
        <div v-html="message"></div>
    </div>
    <!-- <script src="https://cdn.bootcdn.net/ajax/libs/vue/2.6.12/vue.common.dev.min.js"></script> -->
    <script src="./MVVM.js"></script>
    <script>
        const vm = new Vue({
            el: '#app',
            data: {
                user: {
                    name: 'zoulam',
                    age: '18',
                },
                message: `<h1>hello world</h1>`
            },
            computed: {
                getNewName() {
                    return this.user.name + '玩家'
                }
            },
            methods: {
                change() {
                    this.user.age = '100'
                }
            }
        })
    </script>
</body>

</html>
```



### Proxy+Reflect(Vue3.0)

#  koa2中间件的实现

> `app.use` 不涉及**请求方法、请求路径**的判断。
>
> `router` 统一处理路由

## 关于洋葱模型

> 洋葱模型的优势：减少设计模式对业务的入侵
>
> ​	

```javascript
const Koa = require('koa');
const app = new Koa();

// logger

app.use(async (ctx, next) => {
    console.log("1、注册日志中间件");
    await next();
    console.log("6、打印日志");
    const rt = ctx.response.get('X-Response-Time');
    console.log(`${ctx.method} ${ctx.url} - ${rt}`);
});

// x-response-time

app.use(async (ctx, next) => {
    console.log("2、记录响应时间");
    const start = Date.now();
    await next();
    console.log("5、存储响应时间");
    const ms = Date.now() - start;
    ctx.set('X-Response-Time', `${ms}ms`);
});

// response

app.use(async ctx => {
    console.log("3、处理响应……");
    ctx.body = 'Hello World';
    console.log("4、响应完成");
});

app.listen(3000,()=>{
    console.log(`http://localhost:3000`);
});
```

![打印结果](C:/Users/zoulam/AppData/Roaming/Typora/typora-user-images/image-20200812203718603.png)

## 最小实现

```JavaScript
const http = require('http');

// 组合中间件
function compose(midWareList) {
    // 形成闭包
    return function (ctx) {
        // 执行中间件
        function dispatch(i) {
            const fn = midWareList[i]
            try {
                // 实现兼容，防止传入的中间件不是 async函数
                return Promise.resolve(
                    // dispatch.bind(null, i + 1) 这就next的执行机制，
                    // 即取出下一个中间件
                    fn(ctx, dispatch.bind(null, i + 1))
                )
            } catch (error) {
                return Promise.reject(error)
            }
        }

        return dispatch(0)// 初始化内存中闭包
    }
}


class koa2 {
    constructor() {
        this.midWareList = []
    }

    use(fn) {
        this.midWareList.push(fn)
        return this
    }

    handleRequest(ctx, fn) {
        return fn(ctx)
    }

    createContext(req, res) {
        const ctx = {
            req,
            res
        }

        // 实际源码
        // ctx.query = req.query ……
        return ctx;
    }

    calllback() {
        const fn = compose(this.midWareList)
        return (req, res) => {
            const ctx = this.createContext(req, res)
            return this.handleRequest(ctx, fn)
        }
    }

    listen(...args) {
        const server = http.createServer(this.calllback())
        server.listen(...args)
    }
}

module.exports = koa2
```

## 演示代码

```javascript
const koa2 = require('./koaDemo');
const app = new koa2();

// logger

app.use(async (ctx, next) => {
    console.log("1、注册日志中间件");
    await next();
    console.log("6、打印日志");
    // const rt = ctx.response.get('X-Response-Time');
    const rt = ctx['X-Response-Time'];
    // console.log(`${ctx.method} ${ctx.url} - ${rt}`);
    console.log(`${ctx.req.method} ${ctx.req.url} - ${rt}`);
});

// x-response-time

app.use(async (ctx, next) => {
    console.log("2、记录响应时间");
    const start = Date.now();
    await next();
    console.log("5、存储响应时间");
    const ms = Date.now() - start;
    // ctx.set('X-Response-Time', `${ms}ms`);
    ctx['X-Response-Time'] = `${ms}ms`;
});

// response

app.use(async ctx => {
    console.log("3、处理响应……");
    ctx.res.end('Hello World');
    console.log("4、响应完成");
});

app.listen(3000, () => {
    console.log(`http://localhost:3000`);
});
```

# express中间件原理

> app.use用来注册中间件，先收集起来【判断类型，string/callback】
>
> 遇到http请求，根据path和method判断触发哪些【if else】
>
> 实现next机制，即上一个通过next触发下一个【yield】

> **代码流程**
>
> ​	1、处理传入的参数，根据分割成`路由`和`中间件`，
>
> ​	2、做出路由匹配并处理，
>
> ​	`match`:实现路由命中规则，【只要包含就能命中】
>
> 	`handle`  ：实现next机制，递归使用，直到中间件函数中不存在`next()`
>
> ​	`callback` :设置http请求，包括：解析cookie、解析session、解析json【res.json()】……
>
> ​	`listen`：创建http服务器并监听

```javascript
const http = require('http');
const slice = Array.prototype.slice;


class LikeExpress {
    constructor() {
        this.routes = {
            all: [], // app.use注册
            get: [], // app.get
            post: [] // app.post
        }
    }

    // 通用注册函数
    register(path) {

        // info.path 用于接收传入的路由参数
        // info.stack 用于存放中间件
        //  然后再push到相应的routes数组中
        const info = {}

        if (typeof (path) === 'string') {// 有路由时
            info.path = path;
            // 从第二参数开始，转换为数组，存入stack
            info.stack = slice.call(arguments, 1);
        } else {// 无路由时
            info.path = '/';
            // 从第一参数开始，转换为数组，存入stack
            info.stack = slice.call(arguments, 0);
        }
        return info;
    }

    // use get post 使用register函数，对传入的参数做出处理，将路由和中间件函数分割
    use() {
        const info = this.register.apply(this, arguments)
        this.routes.all.push(info)
    }

    get() {
        const info = this.register.apply(this, arguments)
        this.routes.get.push(info)
    }

    post() {
        const info = this.register.apply(this, arguments)
        this.routes.post.push(info)
    }

    // 匹配中间件
    match(method, url) {
        let stack = [];
        if (url === 'favico.ico') {//网页图标
            return stack//不处理
        }

        // 获取当前routers
        let curRoutes = [];
        // use注册中间件所有网页都需要处理
        curRoutes = curRoutes.concat(this.routes.all)
        // 根据请求的方法拼接内容
        curRoutes = curRoutes.concat(this.routes[method])

        // 传入的url与路由相同
        curRoutes.forEach(routeInfo => {
            if (url.indexOf(routeInfo.path === 0)) {
                // url === '/api/get-cookie' routeInfo.path === '/'
                // url === '/api/get-cookie' routeInfo.path === '/api'
                // url === '/api/get-cookie' routeInfo.path === '/api/get-cookie'
                stack = stack.concat(routeInfo.stack)
            }
        })
        return stack

    }


    // 实现next机制
    handle(req, res, stack) {
        const next = () => {
            // 拿到第一个匹配的中间件
            // 一直执行，知道midWare为空
            let midWare = stack.shift()
            if (midWare) {
                // 递归实现
                // 中间件的参数中也传入next函数
                // 执行中间件的时候会再次执行next函数
                midWare(req, res, next)
            }
        }
        next();
    }

    // callback ，这里类似于前面无框架开发的app.js做的事情，实际内容肯定不止这些
    // 1、设置http请求 （解析cookie、解析session、解析json【res.json()】）
    // 2、是判断路由和请求，然后执行  handle
    // 3、处理完成的next流程
    callback() {
        return (req, res) => {
            // 自定义json解析函数
            res.json = (data) => {
                res.setHeader('Content-type', 'application/json')
                res.end(JSON.stringify(data))
            }

            const url = req.url;
            const method = req.method.toLowerCase();

            // 解析请求和url
            const resultList = this.match(method, url)

            // resultList 中间件实体 数组形式
            this.handle(req, res, resultList);
        }
    }

    listen(...args) {
        const server = http.createServer(this.callback())
        // 遵从node.js原生的http
        server.listen(...args)
    }
}

// 工厂函数
module.exports = () => {
    return new LikeExpress();
}
```



