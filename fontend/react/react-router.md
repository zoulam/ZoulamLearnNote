# \[react\]router

![BrowserRouter的props](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201024224139008.png)

```text
主要是 history、location、match
```

# 1、基本语法

```
npm install react-router-dom --save 【包含react-router】
```

[官网](https://reactrouter.com/web/api/Redirect/to-string)

## `<BrowserRouter>`

| 字段（类型）       | 功能 |
| ------------------ | ---- |
| basename（string） | 拼接 |

## `<Link>` 

> 本质是对a标签的封装，访问url

| 字段（类型）                              | 功能                             |
| ----------------------------------------- | -------------------------------- |
| to(string)                                | 点击跳转的url                    |
| to(object) `{pathname,search,hash,state}` |                                  |
| to:(Function)                             |                                  |
| replace:(boolean)                         |                                  |
| other                                     | 传入 `title`  、 `className`字段 |

## `<Switch>`

> 独占路由

| 字段（类型）                 | 功能 |
| ---------------------------- | ---- |
| location:（object）          |      |
| children:（React.Component） |      |

## `<Route/>`

> ​	渲染方式中component传入回调函数，react会认为每一次的回调函数都是不一样的，会反复 `mount`和`unMount`，**出现性能问题**。

| 字段（类型）                                | 功能                                           |
| ------------------------------------------- | ---------------------------------------------- |
| path:（string）                             |                                                |
| exact                                       | 严格匹配                                       |
| child:(React.Component\|\|function)         | **最高优先级**                                 |
| component:（React.Component \|\| function） | 传入function能运行但是效率低（**次等优先级**） |
| render:(function)                           | **最低优先级**                                 |
| replace:(boolean)                           | true 的时候，不保存历史【即网页无法后撤】      |
| strict:boolean                              |                                                |
| sensitive【中文释义：敏感的】:boolean       |                                                |

<img src="https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201005161236677.png" alt="对比" style="zoom: 67%;" />

### Route渲染方式源码解释

<img src="https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201005161859053.png" alt="源码解释优先级" style="zoom:67%;" />

## `<Redirect/>`

| 字段（类型）                                               | 功能                                          |
| ---------------------------------------------------------- | --------------------------------------------- |
| to:(string)                                                | 重定向的路径                                  |
| to:(object)包含`pathname（字符串）`和`state（自定义对象）` | state会被传入到重定向组件的 props的location上 |
| push:(boolean)                                             |                                               |
| from:(string)                                              |                                               |
| exact:(boolean)                                            |                                               |
| strict:(boolean)                                           |                                               |

```javascript
<Redirect to={{ pathname: "/login", state: { redirect: path } }} />
```

# 2、基本使用

**注：** 默认重命名`BrowserRouter` 为 `Router`

```javascript
import { BrowserRouter as Router, Route, Link, Switch } from 'react-router-dom'
```

```javascript
【路由不匹配时children的match为null，Route children使用方式：传入函数，函数的参数包含match】
```

## 嵌套路由和动态路由

>  ​	如果对嵌套路由路由开启 `exact`会导致后面的**匹配失败**。
>
> ​	可以从被嵌套的组件`props`可以解析出动态路由的参数，再根据动态路由渲染不同的页面。

```javascript
<Router>
    <Link to="/user/123456">user</Link>
    <Switch>
        <Route path="/user/:userid" component={UserComponentPage} />
        <Route path="/" children={() => <div>404</div>} />
    </Switch>
</Router>

function UserComponentPage(props) {
    const { userid } = props.match.params
    return (
        <>
            <div>动态路由</div>
            {userid}
            <Link to={"/user/" + userid + "/detail"}>详情页</Link>
            <Route path={"/user/:" + userid + "/detail"} component={DetailComponent} />
        </>
    )
}

function DetailComponent(props) {
    return (<div>DetailComponent</div>)
}
```

![动态路由的参数](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201101170055117.png)

## 路由守卫\(保护路由\)

> 逻辑分析：未登录的进入用户中心
>
>  1、有`history`跳转`history`
>
>  2、没有`history`就跳转指定页（如：首页） **需要错误处理**
>
> 考点：重定向的内容是存入`location`的
>

![location对象](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201024231608619.png)

```JavaScript
import React, { Component } from 'react'
import { BrowserRouter as Router, Route, Link, Switch, Redirect } from 'react-router-dom'
export default class RouteSyntax extends Component {
    render() {
        return (
            <div>
                <Router>
                    <Link to="/user/123456">user</Link>
                    <Link to="/">home</Link>
                    <Link to="/login">login</Link>
                    <Switch>
                        <PrivateRoute path="/user/:userid" component={UserPage} />
                        <Route path="/" component={HomePage} />
                        <Route path="/login" component={LoginPage} />
                        <Route children={() => <div>404</div>} />
                    </Switch>
                </Router>
            </div>
        )
    }
}


function HomePage() { return (<div>Home</div>) }
function UserPage() { return (<div>User</div>) }

-------------------------保护路由---------------------------------
export class PrivateRoute extends Component {
    render() {
        const { isLogin, component, path } = this.props
        if (isLogin) {
            return <Route path={path} component={component}></Route>
        } else {
            return <Redirect to={{ pathname: "/login", state: { redirect: path } }} />
        }
    }
}

--------------------------------登录页面-------------------------------------
function LoginPage(props) {
    const { isLogin, location } = props
    // 避免直接敲地址导致state为undefined
    const { redirect = "/" } = location.state || {}
    console.log(isLogin);
    if (isLogin) {
        return <Redirect to={redirect} />
    } else {
        return <div>login</div>
    }
}

```

# 3、对比

## 路由对比

|  | BrowserRouter | HashRouter | MemoryRouter |
| :--- | :--- | :--- | :--- |
| 实现url和ui同步方式 | H5 history [`History.pushState()`](https://developer.mozilla.org/zh-CN/docs/Web/API/History/pushState) [`History.replaceState()`](https://developer.mozilla.org/zh-CN/docs/Web/API/History/replaceState) [popstate](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/popstate_event) | 用的[location.hash](https://developer.mozilla.org/zh-CN/docs/Web/API/Location/hash) | （保存在内存的router）用于React-Native非浏览器环境 |
|  | 需要发送请求，服务端配合否则404 | 不需要**服务端渲染** |  |
|  |  | 不支持location.key和location.state |  |
|  |  | url的端口号后面多了`/#/` |  |

## `<Switch/>` exact strict 对比

|      | `<Switch>`                 | eaxct                                           | strict         |
| ---- | -------------------------- | ----------------------------------------------- | -------------- |
|      | 组件                       | 字段                                            | 字段出现在     |
|      | 只渲染第一个被匹配到的路由 | 不开启就是头部匹配，Route的path是Link的to的开头 | 不忽略尾部斜杠 |
| 使用 |                            | 无子路由添加上，有子路给子路由添加              |                |

```javascript
不开启独占路由
--------------/user页面会渲染全部组件-----------------
<Router>
    <Link to="/user">user</Link>
    <Route exact path="/user" component={UserPage} />
    <Route exact path="/user" component={UserButton} />
    <Route exact path="/user" component={UserCard} />
    <Route path="/home/gg" children={()=>{<div>don't show</div>}} />    
</Router>


----------------开启则渲染第一个组件,UserPage-----------------
<Router>
    <Link to="/user">user</Link>
    <Switch>
        <Route exact path="/user" component={UserPage} />
        <Route exact path="/user" component={UserButton} />
        <Route exact path="/user" component={UserCard} />
        <Route path="/home/gg" children={()=>{<div>don't show</div>}} />    
    </Switch>
</Router>
```



```javascript
不开启exact
	<Link to=" /one/two"/>
    <Route path="/one" component={User} />
	<Route path="/one/two" component={User} />
    <Route render={() => <div>404</div>} />
都能匹配成功
```

```javascript
	<Link to=" /one"/>
    <Route strict path="/one/" component={User} />
-----------------------匹配失败不会忽略尾部/---------------------------------        
```



# 4、React-Router的简单实现

## 实现BrowserRouter组件

> ​	`history` 库是来自 `react-router-dom`，基于H5的history api 做了很多兼容性实现
>
> ​	`BrowserRouter`：本质是一个数据仓库，使用`context`向下给`Route`、`Redirect`传递 `history`和`location` `match`
>
> ​	使用了以下api:`history.listen(callback)`用于监听`history`的变化，返回值是跟`setTimeout`一样的标记id

```javascript
import React, { Component } from 'react'
import { createBrowserHistory } from 'history'
export const RouterContext = React.createContext();
export default class BrowserRouter extends Component {
    constructor(params) {
        super();
        this.history = createBrowserHistory();
        this.state = {
            location: this.history.location
        };
        this.unlisten = this.history.listen(location => {
            this.setState({ location })
        })
    }
    componentWillUnmount() {
        if (this.unlisten) {
            this.unlisten();
        }
    }
    render() {
        return (
            <RouterContext.Provider value={{ history: this.history, location: this.state.location }}>
                {this.props.children}
            </RouterContext.Provider>
        )
    }
}
```

## 实现Route组件

>  `matchPath`是 `react-router-dom`的库函数，使用正则做path处理
>
> 
>
> 渲染情况分支
>
> 0、match匹配成功
>
> ​	匹配渲染，不匹配渲染 children（**Switch可以剔除这种情况**）,child也不存在就是null 
>
> 1、children存在渲染children
>
> ​	1.1、传入传入 `React.Component`
>
> ​	1.1、传入回调函数
>
> 2、children不存在component存在渲染component
>
> ​	2.1、component传入 `React.Component`
>
> ​	2.2、component传入回调函数
>
> ​				使用 `React.createElement(Component, props)` 创建指定节点
>
> 3、前面两个都不存在render存在，渲染render
>
> ​	3.1、render 传入`React.Component`
>
> ​	3.2、render传入回调函数

```javascript
import React, { Component } from 'react'
import { matchPath } from 'react-router-dom';
import { RouterContext } from './BrowserRouter'
export default class Route extends Component {
    render() {
        return (
            <RouterContext.Consumer>
                {
                    context => {
                        const { path, component, render, child } = this.props;
                        {/* const match = context.location.pathname === path; */ }
                        {/* return match ? React.createElement(component, this.props) : null; */ }
                        const location = this.props.location || context.location;
                        const match = matchPath(context.location.pathname, this.props)
                        const props = {
                            ...context,
                            location,
                            match
                        }
                        // 按照 child component render的顺序执行

                        return (
                            <RouterContext.Provider value={props}>
                                {
                                    match
                                        ? child
                                            ? typeof child === 'function'
                                                ? child(props)
                                                : child
                                            : component
                                                ? React.createElement(component, props)
                                                : render
                                                    ? render(props)
                                                    : null
                                        : typeof child === "function"
                                            ? child(props)
                                            : null
                                }
                            </RouterContext.Provider>
                        )

                    }
                }
            </RouterContext.Consumer>
        )
    }
}
```



## 实现Link组件

>  实质是对HTML5的`<a></a>`标签进行封装，用 `history.push`跳转

```javascript
import React, { Component } from 'react'
import { RouterContext } from './BrowserRouter'

export default class Link extends Component {
    hanleClick = (event, history) => {
        // 避免闪烁
        event.preventDefault()
        // 跳转
        history.push(this.props.to)
    }
    render() {
        const { to, children } = this.props;
        return (
            <RouterContext.Consumer>
                {
                    context => (
                        <a href={to} onClick={(event) => this.hanleClick(event, context.history)}>{children}</a>
                    )
                }
            </RouterContext.Consumer>
        )
    }
}
```



## 实现Switch组件

> ​	只匹配最上面被匹配到的路由
>
> ​	forEach 实现find
>
> ​	不满足条件就没有返回值，满足第一个就给match赋值，后续就不会出现满足条件的了

```JavaScript
import React, { Component } from 'react'
import { matchPath } from 'react-router-dom';
import { RouterContext } from './BrowserRouter'
export default class Switch extends Component {
  render() {
    return (
      <RouterContext.Consumer>
        {context => {
          // match 是是否匹配
          // element 如果匹配的话，返回这个元素
          let match, element;
          const {location} = context;
          // 只有第一个满足条件的才会forEach返回，后面match就不是空了
          React.Children.forEach(this.props.children, child => {
            if (match == null && React.isValidElement(child)) {
              element = child;
              const {path} = child.props;
              match = path
                ? matchPath(location.pathname, child.props)
                : context.match;
            }
          });
          return match
            ? React.cloneElement(element, {computedMatch: match})
            : null;
        }}
      </RouterContext.Consumer>
    );
  }
}
```



## 实现Redirect组件

> ​	使用`push()`或者`replace()`跳转，使用生命周期触发跳转

```javascript
import React, {Component} from "react";
import {RouterContext} from "./Context";

export default class Redirect extends Component {
  render() {
    return (
      <RouterContext.Consumer>
        {context => {
          const {history, push = false} = context;
          const {to} = this.props;
          return (
            <LifeCycle
              onMount={() => {
                push ? history.push(to) : history.replace(to);
              }}
            />
          );
        }}
      </RouterContext.Consumer>
    );
  }
}


class LifeCycle extends Component {
  componentDidMount() {
    if (this.props.onMount) {
      this.props.onMount.call(this, this);
    }
  }

  componentWillUnmount() {
    console.log("componentWillUnmount", this);
    if (this.props.onUnmount) {
      this.props.onUnmount.call(this, this);
    }
  }
  render() {
    return null;
  }
}
```

