# note1

## note1

## 1、内容

![&#x67B6;&#x6784;&#x793A;&#x610F;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201005221323026.png)

![&#x6280;&#x672F;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201005221536362.png)

### ①相关链接

[electron](https://www.electronjs.org/docs)

 [electronAPI](https://www.electronjs.org/docs/api)

 [BrowserWindow对象](https://www.electronjs.org/docs/api/browser-window)

 [electron事件](https://www.electronjs.org/docs/api/app#event-gpu-info-update)

 [~~devtron开发工具~~](https://www.electronjs.org/devtron)：用于查看进程间的信息的开发工具（插件）

[react](https://zh-hans.reactjs.org/)

 [reacthook](https://zh-hans.reactjs.org/docs/hooks-reference.html)

[脚手架配置的代码规范](https://www.npmjs.com/package/eslint-config-react-app)

 [eslint自主配置文件获取](https://cn.eslint.org/demo/)

[bootstrap](https://getbootstrap.com/docs/4.5/getting-started/introduction/)

[fontawesome](https://fontawesome.com/) 新版本已经使用了SVG实现，这个网站可以查类名

 [react-fontawesome](https://github.com/FortAwesome/react-fontawesome)这里可以查安装和基本使用，此处使用单独引用（**Explicit Import**），他有五个分类，按需获取。

[uuid](https://www.npmjs.com/package/uuid)

[react-simplemde-editor](https://github.com/RIP21/react-simplemde-editor/tree/v4.1.0)

[如何在nodejs中使用es6import语法](https://stackoverflow.com/questions/45854169/how-can-i-use-an-es6-import-in-node)

[nodejs的fs.promises](http://nodejs.cn/api/fs.html#fs_fs_promises_api)

[processon](https://www.processon.com/) 在线绘图工具（五张免费画布）

[whimsical](https://whimsical.com/)原型图绘制工具 里面的 wirframe 绘制原型图还不错

[dogapi](https://dog.ceo/dog-api/)

[catapi](https://docs.thecatapi.com/example-by-category)

### ②环境和依赖版本

`electron^5.0.6`不要使用最新的版本，错误的解决方案还不完备，不过有些地方支持 `promise`语法还是很爽的。

## 2、electron

### ①electron代码结构分析

```bash
app----------------------------应用程序代码目录
├─main.js----------------------程序启动入口，主进程  #（需要在package.json中配置）
├─common-----------------------通用模块
├─log--------------------------日志模块
├─config-----------------------配置模块
├─ipc--------------------------进程间模块
├─appNetwork-------------------应用通信模块
└─browserWindows---------------窗口管理，渲染进程
    ├─components---------------通用组件模块
    ├─store--------------------数据共享模块
    ├─statics------------------静态资源模块
    └─src----------------------窗口业务模块
        ├─窗口A----------------窗口
        └─窗口B----------------窗口
```

### ②进程

#### 进程简介

> electron是以chromium的，**主进程**可以理解为浏览器本身工作的进程，**渲染进程**则是浏览器页面工作的进程。
>
>  好处：某个页面崩溃不会影响其他页面
>
>  坏处：吃内存

![&#x4E3B;&#x8FDB;&#x7A0B;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201005224100708.png)

![&#x6E32;&#x67D3;&#x8FDB;&#x7A0B;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201005224141420.png)

![API&#x652F;&#x6301;&#x5BF9;&#x6BD4;&#xFF0C;&#x8BF7;&#x4EE5;&#x5B98;&#x7F51;&#x4E3A;&#x51C6;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201005224231243.png)

#### 主进程和渲染通信方式

[devtron失败的原因](https://github.com/MarshallOfSound/electron-devtools-installer/issues/130) 我放弃了

> 渲染进程需要主进程的部分API完成工作，大概是：哥帮帮忙。

线程之间可以通过简单的共享内存进行通信，但是进程之间的通信就要使用特殊手段了

**1、IPC**

_interprocess communication_

[ipcRenderer](https://www.electronjs.org/docs/api/ipc-renderer)和 [ipcMain](https://www.electronjs.org/docs/api/ipc-main)

![ipc&#x793A;&#x610F;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201005230712780.png)

**2、remote**

**注：**需要在主窗口将：`enableRemoteModule`设置为 `true`

```javascript
// const { BrowserWindow } = require('electron').remote
const { remote } = require('electron');
const { BrowserWindow } = remote;
```

## 3、Hook入门

> render函数等生命周期函数移除

[reacthook](https://zh-hans.reactjs.org/docs/hooks-reference.html)

[常用hook示例](https://usehooks.com/)

### ①useState

```text
import React, { useState } from 'react'

export default function LikeButton() {
    const [like, setLike] = useState(0);
    const [on, setOn] = useState(true);
    // const [obj, setObj] = useState({ like: 0, on: true })
    return (
        <div>
            {/* //!不改的只要填入，不然会相互干扰，即不要用useState设置复杂的数据 */}
            {/* 比起类组件需要把全部状态塞入 this.state 方便拆分状态 */}
            {/* <button onClick={() => { setObj({ like: obj.like + 1, on: obj.on }) }}>😂{obj.like}</button>
            <button onClick={() => { setObj({ on: !obj.on, like: obj.like }) }}>
                {obj.on ? 'On' : 'Off'}
            </button> */}
            <button onClick={() => { setLike(like + 1) }}>😂{like}</button>
            <button onClick={() => { setOn(!on) }}>
                {on ? 'On' : 'Off'}
            </button>
        </div>
    )
}
```

### ②useEffect

> **注：**在render之前执行第一次

等效在原有类组件中 `componentDidMount` 添加`componentDidUpdate`清除 ，也可以理解为每次`render`都执行一次。

清除副作用`return`一个回调函数是函数体清除

第二个参数用于监听和比对原数据有没有变化，用于提高性能。

[dogapi](https://dog.ceo/dog-api/)

第二个参数

1、传入空数组：即无论如何只执行一次。（理解为`componentDidMount` 执行一次就好了）

 场景：对于频繁变化但是不想执行的情况，

2、`[value]`,当且仅当value变化时执行。

### ③传统共享Component的方式

1、[render-props](https://zh-hans.reactjs.org/docs/render-props.html)

render-props是一个术语，指带 `this.props` \(this指向当前组件\)

2、[HOC](https://zh-hans.reactjs.org/docs/higher-order-components.html)

### ④自定义Hooks

#### 命名

`useXxx`

#### 作用

自定义hook不返回新的组件，而是返回处理后的数据，实现state处理。

#### 规则

1、可以使用其他hook进行堆叠，可以调用自定义hook和原生hook。

2、只在最顶层调用hook，不能再循环，条件语句中调用hook

3、只能再React函数（自定义hook，函数组件）中调用hook

#### 细节

2、因为闭包的原因，每次执行函数返回的state都是第一无二的，互不干预的。

### ⑤其他hook

**useContext** 获取上下文数据 `const value = useContext(MyContext);`

 类组件中的 `React.createContext()` `context.Provider()`

**useReducer** 类似于reudx的Reducer `const [state, dispatch] = useReducer(reducer, initialArg, init);`

当执行Expensive的操作时，可以用用下面两个hook进行调优

**useCallback** 用于性能调优，原理是缓存（memoized）state，减少不必要的render与 `shouldComponentUpdate`可以类比

```text
const [count, setCount] = useState(0);
// 原来的，他是一个this函数，组件render的时候他就会执行
   const addClick = () => {
            let sum = 0;
            for (let i = 0; i < count; i++) {
                sum += i;
            }
            return sum;
        },

// 缓存了count的值，count的值不变就不会重新render
         // 注：count是外面的值
  const addClick = useCallback(
        () => {
            let sum = 0;
            for (let i = 0; i < count; i++) {
                sum += i;
            }
            return sum;
        },
        [count]
    );
```

**useMemo**同上

```text
useCallback(fn, deps)` 相当于 `useMemo(() => fn, deps)
```

**useRef**

缓存一个任何生命周期都能获取的值 **变化不会触发重新render**

 作用：1、记住dom节点

 2、延迟执行也能获取最新的值

```text
    let number = 1;
    number++;
    console.log(number);// 永远是2

    let number = useRef(1);
    number.current++;
    console.log(number.current);// 组件每次更新都+1

// 使用
<div ref={number}></div>
```

## 4、我不认识的单词

|  |  |
| :--- | :--- |
| webpreferences | 页面预设 |
| integration  nodeintegration | 集成  集成node能力 |
| reply | 回答，回应 |
| wirframe | 线框图 |

## 5、开始

### ①需求

![&#x5168;&#x5C40;&#x9700;&#x6C42;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201006112858989.png)

![&#x9700;&#x6C42;&#x5206;&#x7C7B;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201006112943239.png)

![react&#x9759;&#x6001;&#x7248;&#x672C;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201006113108245.png)

### ②环境搭建

![&#x539F;&#x7406;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201006113330181.png)

开发环境启动链接本地端口，生产环境链接本地文件。

`npm i electron -D`

`npm install concurrently cross-env electron-is-dev wait-on nodemon --save-dev`

```javascript
nodemon --watch <file/folder> --exec  \"electron .\"
// 监听 文件/文件夹的变化 执行 electron .的命令， 双引号前的反斜杠是转义


"concurrently": "^5.3.0", // 并发执行命令（输出详细信息，并且可以关闭端口）
"cross-env": "^7.0.2", // 适配环境变量（兼容linux和Windows）
"electron": "^10.1.1",// 开发桌面端的框架
"electron-is-dev": "^1.2.0",// 判断环境，读取不同url
"wait-on": "^5.2.0"// 保证命令的先后顺序
```

#### 脚本解释

`"dev": "concurrently \"wait-on http://localhost:3000 && electron .\" \"cross-env BROWSER=none npm start\" "`

[concurrently](https://www.npmjs.com/package/concurrently)

`concurrently \“electron .\” \"npm start\"` 同时执行两条命令

 出现新的问题：react启动明显比elecron慢，会导致加载白屏，需要刷新electron

[wait on](https://www.npmjs.com/package/wait-on)

`wait-on http://localhost:3000 && electron .` 等待3000端口启动再执行electron

[cross-env](https://www.npmjs.com/package/cross-env)

`cross-env BROWSER=none` 告诉react不打开浏览器

### ③文件结构（src）

[react的建议](https://zh-hans.reactjs.org/docs/faq-structure.html)

```bash
├─components
│      ComponentName.css 
│      ComponentName.js
│
├─hooks
│      useHook.js
│
└─utils #
```

[脚手架配置的代码规范](https://www.npmjs.com/package/eslint-config-react-app)

[eslint自主配置文件获取](https://cn.eslint.org/demo/)

## 6、引入库

### ①样式库

[bootstrap](https://getbootstrap.com/docs/4.5/getting-started/introduction/)

```text
npm i bootstrap -S
```

```text
import 'bootstrap/dist/css/bootstrap.min.css'
```

[gird布局](https://getbootstrap.com/docs/4.5/layout/grid/)

#### css类名规范

1、类名标识放在最左侧，即 `left-panel right-panel`等放在最左侧，避免类名过多是出现无法判断的情况，

2、（**超长类名**）不同功能的类名换行，如flex一行，颜色一行，gird一行

3、bootstrap规定按钮的type要写上button

```markup
        // 容器
        <div className="App container-fluied">
            {/* 行 */}
            <div className="row">
                {/* 左侧 */}
                <div className="left-panel col-3 bg-danger">
                    <h1>this is left-panel</h1>
                </div>
                {/* 右侧 */}
                <div className="right-panel col-9 bg-primary">
                    <h1>this is right-panel</h1>
                </div>
            </div>
        </div>
```

![&#x89C4;&#x8303;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201006141404049.png)

#### 布局设计

大页面用 [gird布局](https://getbootstrap.com/docs/4.5/layout/grid/)，小单元用 [flex布局](https://getbootstrap.com/docs/4.5/utilities/flex/)

#### **用到的bootstrap**

[alert颜色](https://getbootstrap.com/docs/4.5/components/alerts/)

[button颜色](https://getbootstrap.com/docs/4.5/components/buttons/)

[表单美化](https://getbootstrap.com/docs/4.5/components/forms/#form-controls)

[背景颜色](https://getbootstrap.com/docs/4.5/utilities/colors/#background-color)

[gird布局](https://getbootstrap.com/docs/4.5/layout/grid/)

[flex布局](https://getbootstrap.com/docs/4.5/utilities/flex/)

[缩写](https://getbootstrap.com/docs/4.5/utilities/spacing/)

[gutters](https://getbootstrap.com/docs/4.5/layout/grid/#gutters)清除空隙 no-gutters

[tabs](https://getbootstrap.com/docs/4.5/components/navs/#tabs)

[rounded-circle](https://getbootstrap.com/docs/4.5/migration/#images) 小圆点

### ②FontAwesome

[fontawesome](https://fontawesome.com/) 新版本已经使用了SVG实现，这个网站可以查类名

 [react-fontawesome](https://github.com/FortAwesome/react-fontawesome)这里可以查安装和基本使用，此处使用单独引用（**Explicit Import**），他有五个分类，按需获取。

[uuid](https://www.npmjs.com/package/uuid)

### ③[prop-types](https://zh-hans.reactjs.org/docs/typechecking-with-proptypes.html)

类型检查

## 7、设计静态页面

1、实现页面基本布局，然后将回调向外暴露

### ①搜索组件

![&#x641C;&#x7D22;&#x7EC4;&#x4EF6;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201006121153245.png)

#### 自定义hook封装键盘行为

### ②文件列表组件

![&#x6587;&#x4EF6;&#x5217;&#x8868;&#x7EC4;&#x4EF6;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201006133212115.png)

#### 文件列表数据设计

![&#x6587;&#x4EF6;&#x6570;&#x636E;&#x7ED3;&#x6784;&#x521D;&#x6B65;&#x8BBE;&#x8BA1;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201006134822242.png)

#### 文件数据结构初步设计

![&#x6587;&#x4EF6;&#x6570;&#x636E;&#x7ED3;&#x6784;&#x521D;&#x6B65;&#x8BBE;&#x8BA1;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201006134937337.png)

#### 样式

[list-group](https://getbootstrap.com/docs/4.5/components/list-group/)

### ③TabList组件

![&#x72B6;&#x6001;&#x5206;&#x6790;](C:/Users/zoulam/Desktop/image-20201006150834840.png)

#### 属性

状态提升到父组件中，以参数传入

![&#x5C5E;&#x6027;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201006150716642.png)

![&#x5176;&#x4ED6;&#x601D;&#x8DEF;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201006151119158.png)

**注：**第二种设计思路是让文件携带相关信息，但是这些信息不是本身携带的，添加的时候就很麻烦

#### 样式

[tabs](https://getbootstrap.com/docs/4.5/components/navs/#tabs) nav-pills

#### 依赖

[类名拼接器](https://www.npmjs.com/package/classnames)

[github地址](https://github.com/JedWatson/classnames)

```text
npm install classnames --save
```

#### 引入scss

[cra文档](https://create-react-app.dev/docs/adding-a-sass-stylesheet/)

### ④引入MDE组件

[react-simplemde-editor](https://github.com/RIP21/react-simplemde-editor/tree/v4.1.0)

[可选参数options](https://github.com/Ionaru/easy-markdown-editor#configuration)

```text
npm install --save react-simplemde-editor
```

**至此静态版本完成**😃

