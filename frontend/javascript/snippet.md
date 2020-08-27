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

```JavaScript
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

## 防抖、节流函数

> 防抖：
>
> ​        1、事件触发n秒后才执行的回调，延迟执行
>
> ​        2、在n秒内再次触发事件，重新计时
>
> ​        3、目的：减少事件触发次数，避免出现抖动
>
> ​        4、场景：ajax提交：首次提交立即执行，从第二次开始使用防抖
>
> ​          	 		 输入验证
>
> ​           			 下拉刷新
>
> ​						常见的提示：您的操作过于频繁请稍后再试

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
> ​	输入验证
>
> ​	悬停出现子菜单
>
> ​	搜索框中的ajax请求
>
> ​	记录用户行为，如：用户在页面的某处停留了多久

```JavaScript
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

## 手写双向绑定

### Object.defineProperty

### Reflect

