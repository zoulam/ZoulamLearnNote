# \[react\]基础

## 0、ReactAPI遍历

![&#x9876;&#x5C42;api](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201024162907426.png)

```react
顶层api(使用React.xx可以使用的，带Symbol的使用方式都是组件)
    Children
        上面挂载了丰富的处理 this,props.children的方法
            .map
            .forEach
            .count
            .only
            .toArray

    Component
        类组件的祖先
    PureComponent
        纯组件，比类组件再更新前多了一个浅比较(类似于shouldComponentUpdate)
    Fragment(Symbol)
        文档碎片等效于：<></>
    memo(React内置的高阶组件,memo是缓存的意思,复用最近一次渲染好的组件)
           const MyComponent = React.memo(function MyComponent(props){})
    Proiler(Symbol) 【分析器，测量渲染代价，**开发模式下生效**】
        <React.Proiler id="xx" onRender={callback}></React.Proiler>
        [onRender详细介绍](https://zh-hans.reactjs.org/docs/profiler.html#gatsby-focus-wrapper)
    StrictMode(Symbol)
        严格模式，**仅在开发模式下生效**
            <React.StrictMode>此范围内的组件（含子组件）会是严格模式，有大量的错误提示</React.StrictMode>
    ~~Suspense(Symbol)~~（中文意思是悬念的意思）
    范围内的组件会出现 loading

    createContext({上下文数据默认值，下面的value空才使用}) 返回一个上下文,上面挂载了Provider、Consumer【适用于不确定层级的传值，最明显的场景就是框架或库开发】
    
	createElement
    createElement(type, [props], [...children])

    ~~createFactroy~~ **被cretaeElement代替**
    cloneElement
        cloneElement(element, [props], [...children])
        只保留element的key和ref，其他同名的被props覆盖，或者新增
    this.inputRef = React.createRef()
        <input ref={this.inputRef}/>
        可以理解为一个引用标记，是所有声明周期函数都能取得的值
    forward（向前）Ref [forward使用](https://juejin.im/post/6844903791456681991)

        const forWardRef((props, ref)=>())
    isValidElement(object)
        判断是不是React对象，返回boolean
    lazy(没有使用就不会被加载进来)
        const SomeComponent = React.lazy(() => import('./SomeComponent'));

    useContext
        const Context = createContext({上下文数据默认值，下面的value空才使用})
        在<Context.Provider>范围内使用
            const value = useContext(Context)


    **useState**
    const [state, setState] = useState(initState)
        setState(newState)
    **useEffect**
        第一个参数是回调函数，添加副作用，回调函数的返回值也是一个回调函数清除副作用，
        第二参数是数组：空数组：只执行一次，非空数组：监听里面的内容
       **useRef**     
       const ref = useRef(null)
        <input ref={this.inputRef}/>
        ref.current.focus()
    useImperative(必要的)Hanle
        useImperativeHandle(ref, createHandle, [deps])

hooks的性能优化
    useMemo（减少重复渲染，下面表示监听a和b的值，发生变化才重新渲染并缓存）
        const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
    useCallback（理解成语法糖）
           useCallback(fn, deps) 相当于 useMemo(() => fn, deps)

    useLayoutEffect

    useDebugValue

    useReducer

    version

    组件的关键值
        从父组件传下来的 <Child [...props]>{children}</Child>
                        this,props
                        this.props.children
        自己的 this.state 使用setState()修改

默认值和静态类型检查
<componentName>.defaultProps = {}
<componentName>.propTypes【这是一个包】 = { name: PropTypes【这是一个对象】.string }

生命周期(记忆方式 component + Will / Did)
    16.3之前有多个will,
    16.3删除3个will，
    16.4getDerivedStateFromProps管得更宽（setState()和forceUpdate()都会干预）

    挂载时
        (constructor)
        默认值 => 静态类型检查
        static getDerived(从……衍生)StateFromProps(props, state)
        render
        React 更新 DOM 和 refs
        componentDidMount

    更新时
        static getDerived(从……衍生)StateFromProps(props, state)
        shouldComponentUpdate(nextProps, nextState) 【props和state变化】
        render
        getSnapshotBeforeUpdate(prevProps, prevState)
        React 更新 DOM 和 refs
        componentDidUpdate

    卸载时 componentWillUnmount()

react模块做了什么？
    React
        解析JSX语法
    react-dom
        将虚拟dom转化为真实dom插入到页面中
```

### context

#### 使用方式一contextType

> 用于类组件，只能订阅一个context【后续订阅会被覆盖前面订阅的】

```react
//------------------------context-----------------------------
// 创建者,填入默认值防止错误
export const ThemeContext = React.createContext({ themeColor: 'pink' });
// 接收者 批发
export const ThemeProvider = ThemeContext.Provider;
// 消费者
export const ThemeConsumer = ThemeContext.Consumer;

//------------------------传入-----------------------------
【注】：出于性能考虑，value应该提升到this.state上（否则会出现重复渲染）    
import {ThemeProvider}
export default class MyComponent{
    constructor(){
        super()
        this.state = {
            theme: {
                themeColor: 'red'
            },
            user: {
                name: 'zoulam'
            }
        }
    }
	render(){
        const { theme } = this.state
        return (
		<>
            {/* <ThemeContext value={{themeColor:"red"}}>*/}
            <ThemeContext value={theme}>    
        		<ThemeContext>
               		<SingleContext />
                </ThemeContext>
			</ThemeContext>  
        </>
        )
    }
}

//------------------------使用-----------------------------
【使用】：class.contextType = context 或 static.contextType = context
class SingleContext{
    static.contextType = ThemeContext
	render(){
		return (
            <>
            	{this.context.themeColor}
            </>
		)
	}
}
SingleContext.contextType = ThemeContext
```

#### 使用方式二Consumer

> 适用于函数组件，可以注册多个context

```react
//------------------------context1-----------------------------
// 创建者,填入默认值防止错误
export const ThemeContext = React.createContext({ themeColor: 'pink' });
// 接收者 批发
export const ThemeProvider = ThemeContext.Provider;
// 消费者
export const ThemeConsumer = ThemeContext.Consumer;

//------------------------context2-----------------------------
export const UserContext = React.createContext({ name: 'lala' });
export const UserProvider = UserContext.Provider;
export const UserConsumer = UserContext.Consumer;
//------------------------传入----------------------------- 
import {ThemeProvider, UserProvider} from xx
export class MyComponent{
    constructor(){
        super()
        this.state = {
            theme: {
                themeColor: 'red'
            },
            user: {
                name: 'zoulam'
            }
        }
    }
	render(){
        const { theme, user } = this.state
        return (
			<>
                <ThemeProvider value={theme}>
                    <UserProvider value={user}>
                        <MultipleContextPage />
                    </UserProvider>
                </ThemeProvider>
            </>
        )
    }
}

//------------------------传入----------------------------- 
export default function MultipleContextPage(){
	return (
		<>
        	<ThemeConsumer>
                {
                theme => (
                    <UserConsumer>
                        {user => <div className={theme.themeColor}>{user.name}</div>}
                    </UserConsumer>
                )
                }
        	</ThemeConsumer>
        </>
    )
}
```

### HOC

> ​	生肉 => (工厂) =>肉罐头
>
> ​	传入组件 + 数据【切块，加热】 =>(工厂加工)=>返回组件

```react
const foo = OldComponent => props => {
    return (
		<>
        	<NewComponent>
        </>
    )
}
```

## 怎样才算掌握了React

引用改作者在知乎[如何考察候选人的react技术水平？](https://www.zhihu.com/question/60548673)问题下的回答

> 热身：
>
> 1. 怎么理解 react 传达组件的概念，react 是 view 么，怎么看 state 的设计
> 2. 兄弟组件的状态怎么互传，有哪些方法
> 3. state 的设计为什么是异步的，同步设计有没有问题
> 4. ssr 会有什么性能问题，哪些会引起内存泄露，引入 redux 后怎么处理请求的逻辑
>
> 正式：
>
> 1. 怎么抽象一个带搜索，单多选复合，有请求的 Selector，区分 smart 和 dumped。如果我再往上加功能，比如 autocomplete 等
> 2. 怎么实现对表单的抽象，数据验证怎么统一处理
> 3. 用 react 来实现一个可视化编辑器的引擎，怎么设计，怎么抽象与 model 的交互，再引入 redux 呢，怎么支持第三方组件热插拔
> 4. 用 react 和 redux 模拟多人协作的 Todo，node 作为后端，怎么设计
>
> 随便想了一些，我不会特别关注库的使用，候选人很精通 angular 和 vue 也很好啊，重点考察对组件的理解，分层，怎么去耦合，MVVM/MVI 现实等。只对 react 用法很熟，那是真码农了。工程师一定能一通百通
>
> 作者：流形 链接：[https://www.zhihu.com/question/60548673/answer/177682784](https://www.zhihu.com/question/60548673/answer/177682784) 来源：知乎 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

## react做了什么

通过webpack配置babel编译jsx等

react: 数据=&gt; VDOM \(处理jsx，即**只要使用了jsx语法就需要引入react**\)

react-dom:VDOM=&gt;DOM

## create-react-app做了什么

[cra文档](https://create-react-app.dev/docs/documentation-intro)

主要是完成了繁琐的webpack配置

1、起一个本地服务器

2、封装配置到 `react-script`

3、集成测试框架

## 项目搭建

`npx create-react-app <projectname>`

`npx create-react-app --typescript <projectname>`

`eject进行了webpack配置`:配置文件在：**node\_modules/react-scripts / config**下

也可以使用 `npm run reject`暴露全部配置，这个过程是不可逆的

# 1、jsx

html： `（）`

js： `{}`

变量

函数

jsx对象 `<div></div>`

条件语句 三元运算符和 && 短路

数组 列表渲染，要加key **dom diff的时候需要比对：先比对type，再比对key**

属性 `<img src="{path}">` `<style="{style}">` `let style = {width:100px,height:200px}`

模块化 react实现了

# 2、组件

> 以函数的形式书写，通过传入的参数自定义化组件内容，组件拥有`状态`和`生命周期`

## ①class组件

继承自 `Component`实现于`render`函数

```react
class MyComponent extends Component {
	render(){
		return (
            <>
            </>
        )
	}
}
```

`this.state`设置 `this.setstate()`设置，设置可能是异步也可能是同步

### **setState**

> 合成事件是**异步**的属于批量更新，大量setState时性能较好
>
> 在原生事件和setTimeout中是**同步**的

`setState(partialState,callback)`

partialState  【partial：中文释义是局部的意思】

是一个对象，或者函数返回值是一个对象（这种方式能实现链式调用）

callback

当state发生变化时执行，使用回调修改是**同步**的

### 生命周期

#### 16.3之前

![16.3](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/88e11709488aeea3f9c6595ee4083bf3)

初始化

1、默认值和静态类型检查

2、构造器执行

3、~~componentWillMount\(\)~~ 将要挂载

4、`render()`

5、`componentDidMount()` 已经挂载

**运行时**

1.1、`componentWillUnmonut()` 直接卸载

1.2、`shouldComponentUpdate(nextProps, nextState)` 更新

​		此处可以做出优化，返回`false`就不会更新

1.2.1 ~~componentWillUpdate~~\(\) 更新

`render()`

`componentDidUpdate()` 更新完成

1.2.2 return false 回到**运行时**

1.3 ~~componentReceiveProps~~\(nextProps\) 接收到新的props\(初次渲染不执行\)

跳转1.2

过期的生命周期加上 `UNSAFE_`

批量修改，不填路径就是整个项目

`npx react-codemod rename-unsafe-lifecycles <path>`

#### 16.3

[新](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)

![preview](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/v2-610ad32e1ed334b3b12026a845e83399_r.jpg)

`static getDeriedStateFromProps(props, state)` 

​	derived：中文释义是从……导出

`getSnapshotBeforeUpdate(preProps, preState)`

获取更新前的缩影【快照】，返回值将会传入 `componentDidUpdate(preProps, preState, snapshot)`

#### 16.4

`getDrivedStateFromProps`管的更宽了，`setState()` 和 `forceUpdate()` 都监听

## ②function组件

> 组件内的状态通过参数传入，使用hook管理state

```react
function MyComponent({props}){
	return (
        <>
        	{props}
        </>
    )
}
```

### hooks

> 剔除生命周期，和render\(\)函数

useState

useEffect

执行setState

监听

清除副作用

#### 自定义hook

#### 使用规则

1、hook和自定义hook一定要是最外层使用，即：不能再循环，条件语句，或者在子函数中调用

```react
    if(true){
        const [count, setCount] = useState(0);
    }
```

2、只有在**React组件**和**自定义hook**中使用hook

#### useMemo

> 减少数据变化没有关系的函数执行，通过**记忆/缓存**的方式，返回回调函数的返回值

#### useCallback

> 减少数据变化没有关系的函数执行，通过**记忆/缓存**的方式，返回**函数**

`useCallback(fn, deps)` 相当于 `useMemo(() => fn, deps)`

## **③组件复合**

> 共用部分内容，如顶部栏和底部栏，与vue的 `<slot></slot>`概念类似

组件内包裹的内容 默认在 `this.props.children`上，传入jsx渲染，或者传入丰富的对象信息

```react
----------------------Layout写法-----------------------------
import React, { Component } from 'react'
import BottomBar from './BottomBar'
import TopBar from './TopBar'

export default class Layout extends Component {
    componentDidMount() {
        const { title } = this.props;
        document.title = title;
    }
    render() {
        const { children, showTopBar, showBottomBar } = this.props;
        console.log(this.props.children);
        return (
            <div>
                {showTopBar && <TopBar></TopBar>}
                {children.content}
                {children.text}
                <button onClick={children.btnClick}>button</button>
                { showBottomBar && <BottomBar></BottomBar>}
            </div>
        )
    }
}

--------------------------Layout使用---------------------------------------
{/* props从此处传入 */}            
<Layout showTopBar={false} showBottomBar={true} title='首页'> 
    {/* Layout包含的内容就是prop.children */}
    {
        {
            content: (
                <div>
                    <div>HomePage</div>
                </div>
            ),
            text: 'this is a text',
            btnClick: () => { console.log('btn click') }
        }
    }
</Layout>
```



## ④redux

![redux-data-flow](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/20181005205138574)

#### reducer的理解

reducer是一个纯函数，执行过程`Array.reduce`类似

功能： `(currentState, action) => newState`

**reducer的限制**

`npm i redux -S`

[redux的理解](https://www.zhihu.com/question/41312576/answer/90782136)

> 之前实现状态共享：
>
> 1、放在顶层组件
>
> 2、状态提升到最近的公共祖先，再传递
>
> 组件间实现状态共享，原理是使用数据仓库\(**store**\)

![redux数据流](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/v2-1111b098e354c2214f137017c92449df_b.webp)

#### 需要使用的情景

* UI 可以根据应用程序状态显着变化
* 并不总是以一种线性的，单向的方式流动
* 许多不相关的组件以相同的方式更新状态
* 状态树并不简单
* 状态以许多不同的方式更新
* 您需要能够撤消以前的用户操作

#### react-redux

```text
npm i react-redux -S
```

provider:为后代组件提供store `<Provider><father></Provider>`

connect：为组件提供数据变更的方法

**需要两次执行**

`connect()(class Component)`

## ⑤react-router

> 根据不同的url渲染不同的页面

`react-router`包含三个库：`react-router` 、`react-router-dom`、`react-router-native`

安装 `react-router-dom` 即可，里面包含了 `react-router`

```text
npm i react-router-dom -S
```

**都是组件化的使用规则**

#### 渲染方式

优先级：children （**不与path匹配，即所有页面可见，覆盖当前页的低优先级组件。** ）&gt;组件渲染&gt;render（三者互斥）

```react
children={() => <div>children</div>}
```

## ⑥其他

### PureComponent（纯组件）

类组件值没有改变也会重新 `render`，PureComponent就内置阻止这种行为，但这知识**浅比较**，对于深层对象无效

缺少生命周期函数`shouldComponentUpdate()`

```react
    shouldComponentUpdate(nextProps, nextState) {
        return nextState.value !== this.state.value;
    }
```

### [Portals（传送门）](https://zh-hans.reactjs.org/docs/portals.html)

> 一种将子节点渲染到非 `root` 节点的方案，是`react-dom`的函数
>
> ​	`ReactDOM.createPortal(child, container)`
>
> ​	child：【展示的ReactComponent】 container ：child插入的文档碎片

```react
export default class Dialog extends Component {
    constructor() {
        super();
        const doc = window.document;
        this.node = doc.createElement('div');
        doc.body.appendChild(this.node)
        this.state = {
            isShow: true
        }
    }
    componentWillUnmount() {
        window.document.body.removeChild(this.node);
    }
    render() {
        const { isShow } = this.state

        return createPortal(
            <div className="dialog">
                {isShow
                    ? <h3>dialog</h3>
                    : null
                }
                {this.props.children}
                <button onClick={() => { this.setState({ isShow: !isShow }) }}>isShow</button>
            </div>
            ,
            this.node
        )
    }
}
```

## ⑦常见问题

### 1、为什么组件必须大写

React程序识别的时候：大写自定义组件，小写原生DOM节点

### 2、

