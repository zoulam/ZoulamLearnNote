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

### Proxy+Reflect(Vue3.0)

