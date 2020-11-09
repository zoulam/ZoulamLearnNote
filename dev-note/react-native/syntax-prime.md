# syntax-prime

关闭警告【删除`.eslintrc.js`文件】

```
module.exports = {
  root: true,
  extends: '@react-native-community',
};
```

与jsx语法的区别语法变动

```javascript
<View></View> 替换 div
<Text></Text> 替换文本
样式
<View style={{fontSize="",color="red"}}></View>
```

# 1、样式

## flex

默认是flex布局，且纵向是主轴

即：`flex-direction:cloumn` 

## 样式继承

RN的样式**不存在继承**，父元素设置的样式不会被继承

## 单位

单位只有数字和百分比，会根据设备调整像素

```javascript
<View style={{width:"50%",height:80}}></View>
```

## [Dimensions](https://reactnative.cn/docs/dimensions#docsNav)

>  **场景：** 深层嵌套的元素需要或许屏幕的百分比

```javascript
import { Dimensions } from 'react-native';
const windowWidth = Dimensions.get('window').width;
const windowHeight = Dimensions.get('window').height;
```

## 变换

对象数组

```
<View style={{ backgroundColor: 'aqua' }}>
<Text style={{ transform: [{ translateX: 50 }, { translateY: 80 }, { scale: 1.2 }] }}>JSX2</Text>
</View>
```

# 2、[内置组件](https://reactnative.cn/docs/components-and-apis)

```javascript
import React from 'react' // 使用了jsx语法就得引入
import {ComponentName} from "react-native"
```

```javascript
<View> // 无法设置字体大小，字体颜色，不能放文本内容，不支持直接绑定点击事件，（TouchableOpacity）
<Text> // 存放文本，支持绑定点击事件
```

## `<TouchableOpacity>`

>  块级元素，用于解决 `<View>` 无法添加点击事件的替代品， **默认点击会有闪烁效果**`

```javascript
function handleClick(){
  alert('hello onpress')
}
const App = () => <TouchableOpacity onPress={handleClick} activeOpacity={0.5} >
  <Text>TouchableOpacity</Text>
</TouchableOpacity>
```

## `<Image>`

### 本地图片

```javascript
 <Image source={require('./images/girl.jpg')} />
```

### 网络图片

> 必须加宽高否则无法显示

```
  <Image style={{width:200,height:400}} source={{uri:"https://th.bing.com/th/id/OIP.fq3C-Dodg9sC0xlCNuB4IQHaLH?pid=Api&rs=1"}} />
```

### gif webp

**android\app\build.gradle** 更改设置【就是安装gradle依赖】

```javascript
dependencies {
    // 让android4.0之前的版本支持gif
    implementation "com.facebook.fresco:animated-base-support:1.3.0"
    // 支持gif
    implementation "com.facebook.fresco:animated-gif:2.0.0"
    // 不包含gif的webp
    implementation "com.facebook.fresco:webpsupport:2.0.0"
    // webp的gif
    implementation "com.facebook.fresco:animated-webp:2.1.0"
}
```

## `<ImageBackground>`

```javascript
  <ImageBackground source={require('./images/girl.jpg')} style={{width:"100%",height:"100%"}}></ImageBackground>
```

## `<TextInput>`

>  **onChangeText**事件，参数是文本信息，而不是事件源，默认是无任何边框样式的 

```javascript
function hanleChange(text) {
  alert(text)
}
const App = () => <View>
  <TextInput onChangeText={hanleChange} />
</View>
```

# 3、插值表达式

```JavaScript
const obj = {
  name: 'zoulam'
}
let arr = ['wukong', 'lala', 'momo']
const App = () => <View>
  <Text>{"fuck"}</Text>
  <Text>{123}</Text>
  <Text>{obj.name}</Text>
  {arr.map((item, index) => <View key={index}><Text>{item}</Text></View>)}
</View>
```

# 4、自定义组件

## 类组件和函数组件

```javascript
class App extends React.Component {
  // 新语法：App.state上
  state = {
    number: 15
  }
  render() {
    setTimeout(() => {
      this.setState({ number: this.state.number + 1 })
    }, 1000)

    return <View>
      <Text>{this.state.number}</Text>
    </View>
  }
}
```



```JavaScript
const App = () => {
  const [number, setNumber] = useState(1)
  setTimeout(() => {
    setNumber(number + 1)
  }, 1000)

  return <View>
    <Text>{number}</Text>
  </View>
}
```

## 插槽和props

```javascript
const App = () => <View>
  <Text>
    -----------------------
  </Text>
  <Sub color="red">
    {
      {
        content: <View></View>,
        text: 'slot text'
      }
    }
  </Sub>
  <Text>
    -----------------------
  </Text>
</View>


const Sub = (props) => <View>
  <Text style={{ color: props.color }}> i am a children</Text>
  {props.children.content}
  <Text>
    {props.children.text}
  </Text>
</View>
```

# 5、**调试**

## ①浏览器调试

> 浏览器**缺点：**无法查看真实的标签
>
> 手机上 `ctrl+m` 进入 `debug` 
>

```
浏览器会自动打开 http://localhost:8081/debugger-ui/
```

```
npm install axios -S
```

```javascript
import axios from 'axios'
axios.get('https://api.github.com/search/users?q=zoulam')
  .then(console.log)
```

[参考解答](https://stackoverflow.com/questions/33997443/how-can-i-view-network-requests-for-debugging-in-react-native) 给项目的 `index.js` 加上下面的内容就可以捕获网络请求 【指network窗口】

```
GLOBAL.XMLHttpRequest = GLOBAL.originalXMLHttpRequest || GLOBAL.XMLHttpRequest;
```

## ②react-native-debugger

[react-native-debugger下载地址](https://github.com/jhen0409/react-native-debugger/releases/tag/v0.11.5)

>  解压出来就能看到可执行文件了
>
>  1、关闭前面的debug `ctrl+m` `stop-debugger`再关闭浏览器
>
>  2、打开工具
>
>  3、再 `ctrl+m` 进入 `debug`  这次就不打开浏览器了

## ③夜神模拟器调试

> 1、使用快捷点 `pagedown`或者使用按钮打开开发者模式
>
> ![按钮](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201108215627266.png)
>
> 2、进去`settings`设置端口号
>
> <img src="https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201108215709308.png" alt="image-20201108215709308" style="zoom: 50%;" />
>
> ```javascript
> 127.0.0.1:8081
> ```
>
> 3、打开 `react-native-debugger` 然后打开 `debug` 模式

# 6、事件

>  this指向

```javascript
class App extends React.Component {
  state = {
    number: 15
  }
  // 2、箭头函数
  // handlePress = () => {
  //   console.log(this.state.number);
  // }

  handlePress() {
    console.log(this.state.number);
  }
  render() {
    return <View>
      {/* <Text onPress={this.handlePress}>click</Text> */}
      {/* 1、箭头函数 */}
      {/* <Text onPress={() => { this.handlePress() }}>click</Text> */}
      {/* 3、bind */}
      <Text onPress={this.handlePress.bind(this)}>click</Text>
    </View>
  }
}
```

# 7、mobx

## ①创建全局数据

```javascript
mobx -S // 核心库
mobx-react -S// react封装之后的库,不包含mobx
@babel/plugin-proposal-decorators // 装饰器语法支持


npm install mobx mobx-react @babel/plugin-proposal-decorators -S
```

`babel.config.js`

```javascript
plugins:[
	["@babel/plugin-proposal-decorators", { "legacy": true }],
]
```

新建`mobx/index.js`

```javascript
// ------------- mobx/index.js------------
import { observable } from 'mobx'
class RootStore {
    @observable
    name = "zoulam";
}

export default new  RootStore()
---------------------下方的代码多了一个export-------------------------------
```

![mobx使用](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201105220919870.png)

## ②修改全局数据

>  `@observer` 监听了但是不重新render
>
>  ​	`mobx-react` 版本过高，新版语法需要使用 `makeObservable`

```JavaScript
数据仓库定义函数
import { makeObservable, action} from 'mobx'
class Store{
	constructor(){
		makeObservable(this)
	}
	@action (){}
}

子组件使用
子组件调用数据仓库定义的@action
@observer【observer一定是最里面一层的装饰器，否则失效】监听数据变化，执行render函数
```

```javascript
export withStyles(styles)(inject('store')(observer(MyComponent)));
```

# 8、错误

[解决方案](https://stackoverflow.com/questions/41802749/the-development-server-returned-response-error-code-500-in-react-native)

```
The development server returned response error code: 500 in react native
```

```
安装的包不在当前项目下，安装到了全局，需要打包的时候没有打包进来，【或者是安装了兼容性太差版本太低的包】
```

