# \[react\]redux

## \[react\]redux

### 1、核心概念

```text
npm install redux -S
```

```text
reducer函数
    参数：action + oldState = 返回值：newState
sotre数据仓库
    huge：state
解决问题
    兄弟组件【不是单指兄弟组件】传输state困难

使用场景
    共用状态：如登录信息，页面主题色调

非复杂场景下可以使用React官方提供的
    context【跨层级通信，禁止滥用，会有数据污染问题】
    hook特有的useReducer
```

![redux&#x6570;&#x636E;&#x6D41;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/v2-1111b098e354c2214f137017c92449df_b.webp)

#### redux

```text
1、创建reducer纯函数、注入store
function counterReducer(state = 0, action) {
    switch (action.type) {
        case 'ADD':
            return state + 1;
        case 'MINUS':
            return state - 1;
        default:
            return state;
    }
}
2、注册：const store = new createStore(`reducer`)
3、订阅：store.subscribe(callback)
        store.getState()
        store.dispatch({ type: 'ADD' })

3.1丰富dispatch功能：如支持异步、打印日志等
声明时：const store = new createStore(counterReducer, applyMiddleware(logger, thunk))
```

```text
import { connect } from 'react-redux';
connect(
    // mapStatetoProps 映射state到props
    // 默认挂载
    state => ({ num: state }),
    // ownProps是本身的属性
    // !需要谨慎使用ownProps这个属性，发生修改mapDispatchToProps会被重新执行，
    // !state也会被重新计算，影响性能
    // (state, ownProps) => {
    //     console.log('ownProps', ownProps);
    //     return {
    //         num: state
    //     }
    // }

    // ? mapDispatchToProps Object||Function
    //  映射dispatch到props
    //
    // {
    //     add: () => ({ type :'ADD'}),
    //     minus: () => ({ type :'MINUS'})
    // }
    // !需要谨慎使用ownProps这个属性，发生修改mapDispatchToProps会被重新执行，
    // !state也会被重新计算，影响性能
    (dispatch, ownProps) => {
        // console.log('ownProps', ownProps);
        let res = {
            // add: () => dispatch({ type: 'ADD' }),
            // minus: () => dispatch({ type: 'MINUS' })
            add: () => ({ type: 'ADD' }),
            minus: () => ({ type: 'MINUS' })
        }
        res = bindActionCreators(res, dispatch)
        return {
            dispatch,
            ...res
        }
    },
    // mergeProps
    // (stateProps, disDispatch, ownProps) => {
    //     return { omg: 'zoulam', ...stateProps, ...disDispatch, ...ownProps };
    // }
)
```

### 2、ReduxFlow

![redux-data-flow](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/20181005205138574)

1. 创建store `createStore(reducer)`

   `constructor` 里订阅 `subscribe`

2. `componentDidMount`
3. 使用：`store.getState()`获取，修改： `dispatch({type:'action'})`

### 3、中间件

> ​ redux是一个纯粹的状态管理器，dispatch只支持传入对象作为实参。
>
> ​ 中间件是一个**函数**，对 `dispatch`进行封装，（让原本只支持传入对象参数，变成支持**回调函数，异步的网络请求，支持日志打印**）。
>
> ​ 先执行原有的 `createStore()`函数，对原有的`dispatch`进行封装，封装过程：将（getState和老的dispatch传入）中间件返回新的`dispatch`。最后保留原有的函数，旧的 `dispatch` 用新的`dispatch`覆盖。
>
> ​ 简单的说就是对 `dispatch`的功能增强。

```text
npm install redux-thunk【thunk中文释义：形实转换程序】 redux-logger redux-saga【也是实现异步的】 --save
```

```text
const store = new createStore(counterReducer, applyMiddleware(logger, thunk))
```

![&#x4F7F;&#x7528;&#x4E2D;&#x95F4;&#x4EF6;&#x524D;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/15827882-0d051633abff2a5f.png)

`dispatch` =\(`midware`\)&gt; `newDispatch`

![&#x4F7F;&#x7528;&#x4E2D;&#x95F4;&#x4EF6;&#x540E;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/15827882-f66b09adbb68334a.png)

### [4、combined-reducers](https://redux.js.org/recipes/structuring-reducers/initializing-state#combined-reducers)

> 多个reducer

```text

```

### 5、redux简单实现

#### 前置知识compose

聚合函数：上一个函数的返回值作为参数传入下一个函数

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

> 对中间件的理解，当注册了中间件之后，后面传入的state就会被中间件拦截，而不是**只被**createStore函数处理

```text
1、createStore函数
    1.1、向外暴露
        return {
            getState, // 返回当前state
            dispatch, // 传入 currentSatate + action 返回新的 state
            subscribe // 订阅，将回调函数push进数组
        }
    1.2、执行dispatch赋初值
2、applyMiddleware // 对dispatch进行加强
    2.2 中间件的执行是使用compose函数进行聚合，所以传入的顺序是有关系的 【通常异步函数会放在后面】
```

```text
export function createStore(reducer, enhanser) {// 【enhanser中文释义：增强因子】
    if (enhanser) {
        // reducer就是中间件
        return enhanser(createStore)(reducer)
    }
    let currentState = undefined;
    let currentListener = [];
    // 获取状态
    function getState() {
        return currentState;
    }
    // 传入数据
    function dispatch(action) {
        currentState = reducer(currentState, action);
        currentListener.map(listener => listener())
    }

    // 订阅仓库,可以有多处组件监听
    function subscribe(listener) {
        currentListener.push(listener);
    }

    // 执行dispatch
    dispatch({ type: '@init/react-redux' });
    // 返回
    return {
        getState,
        dispatch,
        subscribe
    }
}

export function applyMiddleware(...midwares) {
    // ...args 是reducer
    return createStore => (...args) => {
        // console.log(midwares);
        // 初始化无中间件的处理
        const store = createStore(...args);
        // 解析dispatch和getState方法
        let dispatch = store.dispatch;
        const middleApi = {
            getState: store.getState,
            dispatch
        }
        // 给参数注入，比如说dispatch
        const middlewaresChain = midwares.map(middleware =>
            middleware(middleApi)
        );
        // console.log(typeof middlewaresChain[0]);// function
        dispatch = compose(...middlewaresChain)(dispatch);
        return {
            ...store,
            // 覆盖store的dispatch
            dispatch
        }
    }
}


function compose(...funcs) {
    if (!funcs.length) return arg => arg;
    if (funcs.length === 1) return funcs[0];
    return funcs.reduce((a, b) => (...args) => a(b(...args)))
}
```

#### logger的简单实现

```text
export function logger({ getState, dispatch }) {
    return dispatch => action => {
        if (typeof action.type === 'string') {
            console.log(action.type + "执行了")
        }
        return dispatch(action);
    }
}
```

#### thunk的简单实现

```text
export function thunk({ getState, dispatch }) {
    return dispatch => action => {
        // action 可以是对象、函数
        if (typeof action === 'function') {
            return action(dispatch, getState);
        } else {
            return dispatch(action);
        }
    }
}
```

## react-redux

> react是对redux的封装，但是还是需要redux模块

```text
1、 创建reducer函数、注入store

2、注册：<Provider store={store}>

3、高阶组件：connect(callback)(<Component\>)

第一个参数tate => ({ num: state })，后面的括号不是什么奇怪的语法

不写括号：state => {num:state}
大括号不会被识别成对象的大括号，而是函数的{}

4、组件中使用 this.props.dispatch传入action【默认传入】
```

### connect

> **\[\]**表示可选
>
> `<xxx>` xxx变量
>
> 都是注入到 this.props中
>
> : 返回值
>
> （类型）
>
> `(参数)`

`connect(mapStatetoProps, mapDispatchToProps, mergeProps)(OldComponent) : NewComponent`

#### `mapStatetoProps:stateProps`\(Function\)

**参数**：`(state, [ownProps])`

```javascript
-------------------一般写法---------------------------
(state) => ({num: state})

-------------------特殊写法---------------------------
(state, ownProps) => {
    console.log('ownProps', ownProps);
    return {
        num: state
    }
}
```

ownProps:是高阶组件封装之前的旧组件的的属性，更新需要重新计算state， **存在性能问题**。

#### `mapDispatchToProps:dispatchProps`\(Object \| Function\)

> `dispatch`会被**默认注入**到 props中

`Object`覆盖默认的 `dispatch`

`Function`则需要手动包装

**参数**：`(dispatch, [ownProps])`

```javascript
-------------------对象---------------------------
{
   add: () => ({ type :'ADD'}),
   minus: () => ({ type :'MINUS'})
}
-------------------函数---------------------------
import { bindActionCreators } from 'redux';
(dispatch, ownProps) => {
        let res = {
            // add: () => dispatch({ type: 'ADD' }),
            // minus: () => dispatch({ type: 'MINUS' })
            add: () => ({ type: 'ADD' }),
            minus: () => ({ type: 'MINUS' })
        }
        res = bindActionCreators(res, dispatch)
        return {
            dispatch,
            ...res
        }
    },
```

#### `mergeProps:props`\(Function\)

**参数**：`(stateProps, disDispatch, ownProps)`

```javascript
(stateProps, disDispatch, ownProps) => {
        // 合并旧内容，添加新内容
        return { omg: 'zoulam', ...stateProps, ...disDispatch, ...ownProps };
}
```

### react-redux的简单实现

> 简化了redux操作。

```text
1、两个api：connect高阶组件（使用）和 Provider组件（注册）
2、使用context 将Provider组件注入的内容传输到 connect
3、封装原始的redux，包括subscribe、getState，将函数操作封装成变量注入到props
4、bindActionCreator可以将dispatch结构成单独的函数注入到props
    对象自动封装
    函数直接执行，可选择手动封装
```

```javascript
import React, { Component } from 'react'
// import { bindActionCreators } from 'redux';
// import { connect } from 'react-redux'


const ValueContext = React.createContext();
export const connect = (
    // 参数1
    mapStateProps = state => state,
    // 参数2
    mapDispatchToProps
) => WrappedComponent => {
    return class extends Component {
        static contextType = ValueContext;
        constructor() {
            super();
            this.state = {
                props: {}
            };
        }
        componentDidMount() {
            // 此处的context是从 redux的createStore()的返回值
            const { subscribe } = this.context;
            this.update();
            // 订阅
            subscribe(() => {
                this.update();
            })
        }

        update = () => {
            // getState dispatch 都是从原来的redux解析出来的，意思是react-redux只是对原来的redux进行了封装
            const { getState, dispatch } = this.context;
            let stateProps = mapStateProps(getState());
            let dispatchProps;
            if (typeof mapDispatchToProps == 'object') {
                dispatchProps = bindActionCreators(mapDispatchToProps, dispatch);
            } else if (typeof mapDispatchToProps == 'function') {
                dispatchProps = mapDispatchToProps(dispatch, this.props)
            } else {
                // 默认
                dispatchProps = { dispatch }
            }

            this.setState({
                props: {
                    ...stateProps,
                    ...dispatchProps
                }
            });
        }

        render() {
            console.log(this.context);
            return (
                <WrappedComponent  {...this.state.props} />
            )
        }
    }
}


export class Provider extends Component {
    render() {
        return (
            <ValueContext.Provider value={this.props.store}>
                {this.props.children}
            </ValueContext.Provider>
        )
    }
}

// {
//     add: () => ({ type :'ADD'}), ==>  add: () => dispatch({ type :'ADD'})
// }

function bindActionCreator(creator, dispatch) {
    return (...args) => {
        console.log('creator res : \n', creator(...args));// { type :'ADD'}
        return dispatch(creator(...args))
    }
}

export function bindActionCreators(creators, dispatch) {
    const obj = {};
    for (const key in creators) {
        obj[key] = bindActionCreator(creators[key], dispatch)
    }
    return obj;
}
```

