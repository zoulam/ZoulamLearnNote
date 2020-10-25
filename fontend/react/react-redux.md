# \[react\]redux

## \[react\]redux

## compose

聚合函数

```javascript
function func1(arg) {
    console.log('func1', arg);
    return arg;
}

function func2(arg) {
    console.log('func2', arg);
    return arg;
}

function func3(arg) {
    console.log('func3', arg);
    return arg;
}


function compose(...funcs) {
    if (!funcs.length) return arg => arg;
    if (funcs.length === 1) return funcs[0];
    return funcs.reduce((func1, func2) => (...args) => { func1(func2(...args)) })
}

compose(func1, func2, func3)(1);

// func3 1
// func2 1
// func1 1
```

## ReduxFlow

![redux-data-flow](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/20181005205138574)

1. 创建store `createStore(reducer)`
2. `componentDidMount` subscribe
3. 使用：`store.getState()`，修改： `dispatch({type:'action'})`

## redux同步

### 中间件

dispatch自能接收对象参数，想要实现异步需要使用`midware`实现

```text
npm install redux-thunk redux-logger --save
```

中间件对 `dispatch`进行封装，（让原本只支持传入对象参数，变成支持回调函数，支持日志打印），再返回对象类型的`dispatch`

`dispatch` =\(`midware`\)&gt; `newDispatch`

