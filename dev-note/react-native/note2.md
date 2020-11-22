# note2

## note2

## 1、交友页

### ①吸顶效果

> [react-native-image-header-scroll-view](https://www.npmjs.com/package/react-native-image-header-scroll-view)

```text
npm install react-native-image-header-scroll-view -S
```

### ②使用iconfont

1、然后拷贝 ttf后缀的文件到 `android\app\src\main\assets\fonts`中 如果没有`assets`文件夹可以新建一个

```javascript
 <Text style={{ fontFamily: "iconfont", color: "red" }} >{'\ue82b'}</Text>
```

### 一些规范上的警告

> 不按要求会导致样式失效

```text
Valid color formats are
  - '#f0f' (#rgb)
  - '#f0fc' (#rgba)
  - '#ff00ff' (#rrggbb)
  - '#ff00ff00' (#rrggbbaa)
  - 'rgb(255, 255, 255)'
  - 'rgba(255, 255, 255, 1.0)'
  - 'hsl(360, 100%, 100%)'
  - 'hsla(360, 100%, 100%, 1.0)'
  - 'transparent'
  - 'red'
  - 0xff00ff00 (0xrrggbbaa)

Bad object: {
  "fontFamily": "iconfont",
  "fontSize": 15.6,
  "color": "666"
}
```

> 滑动条

```text
npm install @react-native-community/slider -S
```

## 2、

轮播图

```text
npm install react-native-view-overflow  react-native-deck-swiper -S
```

```text
vertical // 垂直
Horizontal // 水平
```

## 1、技巧

```javascript
判断下标是否合法

if(!nums[index]){} // 判断是否存在元素

下标的变化
this.setState({ currentIndex: this.state.currentIndex + 1 })// 修改下标

父元素 position:relative 子元素 position:absolute

// 文本强制不换行,越界变省略号 letterSpacing：文字间隔
<Text  numberOfLines={1} style={{letterSpacing:pxtoDP(8)}} ></Text>

// 重置模式，避免裁切 
<ImageBackground resizeMode="stretch"></ImageBackground>

// 高度拉伸，避免容器未被填满
<ImageBackground imageStyle={{ height: "100%" }}></ImageBackground>


// 随机分布，设置在屏幕范围内的随机数
const tx = Math.random() * (screenWidth - whMap.width)
const ty = Math.random() * (screenHeight - whMap.height)
style={{ position: "absolute", left: tx, top: ty, }}
```

```text
vscode查看json的方式
1、复制json文本
2、ctrl+n创建新文件粘贴到文件内
3、点击右下的选项纯文本选取json类型
4、格式化（shift+ctrl+f）
```

```text
// 常用颜色

"#fff" 白色
”#000“ 黑色
”#ffffff9a“ 略微透明的白色
”#xxxxxx1a“ 后面的1a表示十分透明
"#ccc" 灰色
”#666“ 白灰色
```

## 错误

```text
如果不是从tabbar进入的话会缺少token
```

