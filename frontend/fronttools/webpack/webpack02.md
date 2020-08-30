# webpack02

## webpack02

> 默认cb是`callback`的缩写

## Tapable

> webpack本质是一种事件流机制，将插件串联起来，实现这个功能的核心就是`Tapable`,这类似于`nodejs`内的`events`库,核心原理也是依赖**订阅发布**模式

Tapable在`webpack\lib\Compiler.js`目录下

```javascript
const {
    SyncHook,
    SyncBailHook,
    SyncWaterfallHook,
    SyncLoopHook,
    AsyncParallelHook,
    AsyncParallelBailHook,
    AsyncSeriesHook,
    AsyncSeriesBailHook,
    AsyncSeriesWaterfallHook 
 } = require("tapable");
```

同步注册 `tap` 同步执行`call`

### SyncHook

> 同步钩子，顺序执行

### SyncBailHook

> 保险（bail）钩子，上一次的执行必须返回 `undefined`，否则不会执行后面的函数

### SyncWaterfallHook

> 瀑布钩子，上一个函数的返回值可以被下一个函数以参数的形式接收，产生联系

### SyncLoopHook（webpack中没有使用）

> 遇到特定的钩子（即不会返回 `undefined`）可以循环执行

异步注册 `tapAsync` 异步执行 `callAsync`

或 `tapPromise` 注册 `promise` 执行

## 并行钩子

### AsyncParallelHook

> 异步执行，使用计数器计数，当钩子数执行完之后才执行 `callAsync`内的方法

### AsyncParallelBailHook

> 异步的保险钩子

## 串行钩子（前后有依赖关系）

### AsyncSeriesHook

> 异步的串行钩子，前后有依赖关系

### AsyncSeriesBailHook

### AsyncSeriesWaterfallHook

