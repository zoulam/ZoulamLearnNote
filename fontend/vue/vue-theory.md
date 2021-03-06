---
description: 响应式、diff
---

# \[vue\]原理

## 胡子语法

```javascript
<body>
    <div id="app">{{obj.key.car.color}} <span style="color: red;">{{obj.name}}</span> <strong>{{obj.key.name}}</strong></div>
    <script>
        let obj = {
            val: 15,
            name: 'zoulam',
            key: {
                name: 'lala',
                car: {
                    color: 'red'
                }
            }
        }
        let oApp = document.getElementById('app')
        let val = oApp.innerHTML

        function dep(arr) {
            let [, ...newArr] = arr
            return newArr.reduce((prev, cur) => {
                return prev[cur]
            }, obj)
        }

        function mustache(val) {
            let reg = /\{\{(.+?)\}\}/
            let ans = val.split(reg)
            let target = ans.filter(item => item.startsWith('obj.'))
            let node = target.map(element =>
                dep(element.split('.'))
            );
            let index = -1
            let res = val.replace(reg, (match, lastIndex, oldStr) => {
                index++
                return node[index]
            })
            return res
        }

        oApp.innerHTML = mustache(val)
    </script>
</body>
```

## 响应式

> 四个结构
>
> 1、Dep类 ，实现发布订阅的功能
>
> 2、`reactive`响应式函数 `reactive（data）`，调用4，实现数据的响应式。
>
> 3、观察者类 `new Watcher(effect)` 只要`data`监听的`data`发生变化就执行`effect`
>
> 4、实现响应式的函数 `defineReactive` 使用关键api `defineProperty(obj,key,{get(){},set(){}})`

```javascript
// Dep module
class Dep {
  static stack = []
  static target = null
  deps = null

  constructor() {
    this.deps = new Set()
  }

  depend() {
    if (Dep.target) {
      this.deps.add(Dep.target)
    }
  }

  notify() {
    this.deps.forEach(w => w.update())
  }

  static pushTarget(t) {
    if (this.target) {
      this.stack.push(this.target)
    }
    this.target = t
  }

  static popTarget() {
    this.target = this.stack.pop()
  }
}

// reactive
function reactive(o) {
  if (o && typeof o === 'object') {
    Object.keys(o).forEach(k => {
      defineReactive(o, k, o[k])
    })
  }
  return o
}

function defineReactive(obj, k, val) {
  let dep = new Dep()
  Object.defineProperty(obj, k, {
    get() {
      dep.depend()
      return val
    },
    set(newVal) {
      val = newVal
      dep.notify()
    }
  })
  if (val && typeof val === 'object') {
    reactive(val)
  }
}

// watcher
class Watcher {
  constructor(effect) {
    this.effect = effect
    this.update()
  }

  update() {
    Dep.pushTarget(this)
    this.value = this.effect()
    Dep.popTarget()
    return this.value
  }
}

// 测试代码
const data = reactive({
  msg: 'aaa'
})

new Watcher(() => {
  console.log('===> effect', data.msg);
})

setTimeout(() => {
  data.msg = 'hello'
}, 1000)
```

## 简单mvvm

![vue](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/938664-20170522225458132-1434604303.png)

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
        });
    </script>
</body>

</html>
```

### 最简响应式

```javascript
let a = 3, b = a * 10
// a = 4 , b = 30 // 非响应式
// a = 4 , b = 40 // 响应式
```

> 为什么要重新创建Vue自己的数组原型
> 1、JavaScript的原生数组并没有响应式的能力
> 2、Vue需要响应式，但直接将Array的改造成响应式的会污染原型链

```javascript
let oldArrayPrototype = Array.prototype;
let proto = Object.create(oldArrayPrototype);
['shift', 'push'].forEach(method => {
    proto[method] = function () {//函数劫持
        updateView();// 切片编程
        oldArrayPrototype[method].call(this, ...arguments)
    }
})

// 注册观察者
function Observer(target) {
    if (typeof (target) !== 'object' || target == null) return target;
    if (Array.isArray(target)) {// 拦截数组
        Object.setPrototypeOf(target, proto);
        // target.__proto__ = proto;
    }
    for (const key in target) {
        defineReactive(target, key, target[key])
    }
}

function defineReactive(obj, key, value) {
    Observer(value)
    Object.defineProperty(obj, key, {
        get() {// 收集依赖
            return value;
        },
        set: (newValue) => {
            if (newValue !== value) {
                Observer(newValue)
                value = newValue;
                updateView();
            }
        }
    })
}

function updateView() {
    console.log('更新视图');
}

let data = {
    name: 'zoulam',
    age: {
        n: 18
    },
    nums: [1, 2, 3]
};
Observer(data)
// 深层属性（递归）
data.age.n = 19;
data.age = { n: 100 }
// 新属性（递归）
data.age.n = 19;
data.name = 'luluxi'
// 数组方法需要挂载到对象
console.log('-------------------------------------------');
data.nums.push(4);
console.log(data.nums);
```

# vue-router

>  vue-router本质是基于a标签封装的Vue组件（实现页面跳转和组件按需加载，构建单页面应用的关键）
>
> ​	开发环境下使用：hashHistory
>
> ​	生产环境下使用：基于HTML5的新BOMAPI：history，消除的#
>
> ​	`<router-view/>`直接放在根组件内， `<router-link/>`根据路径加载不同组件