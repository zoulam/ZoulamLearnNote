# \[react\]router

![BrowserRouter&#x4F20;&#x5165;props&#x7684;&#x4FE1;&#x606F;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201024224139008.png)

```text
主要是 history、location、match
```

## 基本使用

[官网](https://reactrouter.com/web/api/Redirect/to-string)

BrowserRouter

​ basename:string 会做拼接

Link

​ to:string

​ to:object `{pathname,search,hash,state}`

​ to:func

​ replace:bool 清空历史

Route

​ exact

​ path:string

​ child:func

​ component:React.Component

​ render:func

Rediect

​ to:string \(this,props.location\)

​ to:object

​ `<Redirect to={{ pathname: "/login", state: { redirect: path } }} />`

```text
【路由不匹配时children的match为null，Route children使用方式：传入函数，函数的参数包含match】
function ListItemLink({to, name, ...rest}){
    return(
        <Route>
            path={to}
            children={({match})=>{
                <li className={match ? "active" : ""}>
                    <Link to={to} {...rest}>
                        {name}
                    </Link>
                </li>
            }}
        </Route>
    )
}


<BrowserRouter>
    <ul>
        <ListItemLink to="" name=""/> 
        <ListItemLink to="" name=""/> 
    </ul>
    <Link to=''></Link> 【用react封装的a标签】
        <Switch>【独占路由】
        【渲染方式的优先级(path相同时)child>component>render】
        <Route exact【严格匹配】 path='' child={callBack}>
        <Route exact path='' component={MyComponent/callBack}> 【传入回调有性能问题】
        <Route exact path='' render={callBack}>
        【动态路由,id不管是什么都是一样的SearchComponent组件，
            但是SearchComponent可以根据，里面根据id渲染不同内容】
        <Route path='/search/:id' component={SearchComponent}>
        <Route path='/' render={<div>404<div>}>
        </Route>
        </Switch>
</BrowserRouter>

function SearchComponent(){
    【结构出上面动态路由传入的id，然后根据不同的id渲染不同的页面】
    const { id } = props.match.params;
    return (
    <div>SearchComponent{id}
            【嵌套路由+动态路由】
            <Link to={"/search/" + id + "/detail"}>详情</Link>
            <Route path={"/search/:" + id + "/detail"} component={DetailComponent}></Route>
    </div>
    )
}
```

## 路由守卫\(保护路由\)

逻辑分析：未登录的进入用户中心

​ 1、有history跳转history

​ 2、没有history就跳转指定页（如：首页） **需要错误处理**

考点：重定向的内容是存入location的

![&#x5B58;&#x5165;&#x503C;&#x5982;&#x4E0B;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201024231608619.png)

```text
<Router>
 <PrivateRouterPage path='/user' component={UserPage} />
</Router>

export default class PrivateRouterPage extends Component {
    render() {
        const { isLogin, path, component【上方传入的UserPage】 } = this.props;
        // 登录就进入正常路径
        if (isLogin) {
            return <Route path={path} component={component} />
        } else {
            // 重定向  
       return 
       <Redirect to={{ pathname: "/login", state: { redirect: path } }} />
           // 会挂载在 this.props.location.state.redirect上
        }
    }
}

export default class LoginPage extends Component {
    render() {
        const { isLogin, location } = this.props;
        // 处理直接敲地址进来的，就是没有设置state的
        const { redirect = '/' } = location.state || {};
        if (isLogin) {
            return <Redirect to={redirect} />
        } else {
            return (
                <div>
                    <h3>LoginPage</h3>
                    // 实现登录和跳转
                    <button onClick={() => { isLogin = true }}>click</button>
                </div>
            )
        }
    }
}
```

## 路由对比

|  | BrowserRouter | HashRouter | MemoryRouter |
| :--- | :--- | :--- | :--- |
| 实现url和ui同步方式 | H5 history [`History.pushState()`](https://developer.mozilla.org/zh-CN/docs/Web/API/History/pushState) [`History.replaceState()`](https://developer.mozilla.org/zh-CN/docs/Web/API/History/replaceState) [popstate](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/popstate_event) | 用的[location.hash](https://developer.mozilla.org/zh-CN/docs/Web/API/Location/hash) | （保存在内存的router）用于React-Native非浏览器环境 |
|  | 需要发送请求，服务端配合否则404 | 不需要服务端渲染 |  |
|  |  | 不支持location.key和location.state |  |
|  |  | url的端口号后面多了`/#/` |  |

![&#x4EE3;&#x7801;&#x89E3;&#x91CA;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201005161236677.png)

### 源码解释

![&#x6E90;&#x7801;&#x89E3;&#x91CA;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201005161859053.png)

