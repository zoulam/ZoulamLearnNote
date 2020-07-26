# css 入门

## 去除默认样式

> 在设置css样式前往往需要去除HTML自带的默认样式

### 简单去除

> 没能将无序列表等样式去除

```css
*{
	margin:0;
	padding:0;	
}
```

### 完全去除

[代码获取](https://github.com/necolas/normalize.css)

## 选择器

| 选择器                                                                                   | 选择内容                                                     |
| ---------------------------------------------------------------------------------------- | ------------------------------------------------------------ |
| `.className`                                                                             | 类选择器                                                     |
| `#id`                                                                                    | id 选择器                                                    |
| `*`                                                                                      | 通配选择器（选择当前页面所有内容）                           |
| `div` `span` ……                                                                          | 直接选取**html**标签名                                       |
| 复合选择器                                                                               |                                                              |
| `div.className` `div#diname`                                                             | 同时满足（and）                                              |
| `A,B`                                                                                    | 单一满足（or）                                               |
| 关系选择器                                                                               |                                                              |
| `A>B`或 `A B`                                                                            | A 是 B 的父亲（选择 B）                                      |
| `A>B>C` 或`A C`                                                                          | A 是 C 的祖宗（选择 C）                                      |
| `A+span`                                                                                 | span 是 A 的第一个兄弟（选择第一个 span，**不包括 A**）      |
| `A~span`                                                                                 | A 后面的全部兄弟（选择 A 的全部 span，**不包括 A**）         |
| 属性选择器                                                                               |                                                              |
|                                                                                          | 通常用来操作不存在的元素（使用 dom 生成的内容）              |
| `p[title]{}`                                                                             | 选择 title                                                   |
| `p[title=abc]{}`                                                                         | 选择 titile=abc                                              |
| `p[title^=abc]{}`                                                                        | 选择 title 以 abc 开头                                       |
| `p[title$=abc]{}`                                                                        | 选择 titile 以 abc 结尾                                      |
| `p[title*=e]{}`                                                                          | 选择 title 包含 e                                            |
| 伪类 一个冒号                                                                            |                                                              |
| `div:first-child`                                                                        | 选择 div 的第一个子元素                                      |
| `xx:last-child`                                                                          | xx 的最后一个子元素                                          |
| `:nth-child(n)`                                                                          | 操作所有子元素                                               |
| `:nth-child(2n)` `:nth-child(even)`                                                      | 操作**偶数**位置的子元素                                     |
| `:nth-child(2n+1)` `:nth-child(odd)`                                                     | 操作**奇数**位置的子元素                                     |
| `:first-of-type`                                                                         |                                                              |
| `:last-of-type`                                                                          |                                                              |
| `:nth-of-type()`                                                                         |                                                              |
| `:not()` `p:not(.fancy) {}`                                                              | 选择类名不是 fancy 的 p 元素                                 |
| a 的伪类（下面的四个属性**存在包含关系**应该从上至下写入不然会失效）                     |                                                              |
| `a:link`                                                                                 | 正常链接                                                     |
| `a:visited`                                                                              | 被访问的链接（出于隐私考虑只能设置颜色属性）                 |
| `a:hover`                                                                                | 选择鼠标移入时（**其他元素也能用**，较常见的就是按钮）       |
| `a:active`                                                                               | 选择鼠标点击未松开时（**其他元素也能用**，较常见的就是按钮） |
| [更多伪类](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes)              |                                                              |
| 伪元素选择器（双冒号）                                                                   |                                                              |
| 用来选择特定位置的**文本信息**内容，比如实现首字母大写，书内的首个单词或文字实现特定样式 |                                                              |
| p::first-letter                                                                          | p 的表示第一个字母                                           |
| p::first-line                                                                            | p 的第一行                                                   |
| p::selection                                                                             | p 内的文本                                                   |
| 实现特定内容包裹如`“”`，`《》`                                                           |                                                              |
| ::after                                                                                  | **after**和**before**必须结合`content`属性使用               |
| div::before{content: '『';}                                                              | 在 div 前面添加『                                            |

## 优先级

`!important > inline > id > class/pseudo class > element > * > inherit`

CSS 优先级森严，同级覆盖关系（根据样式被文档读取的顺序，从上至下），

`!important`往往能不用就不用

## 继承关系

## 样式的继承

不用特别去记忆，只是在出现**未设置样式**异常时应该想到是样式继承的干扰

> 样式继承是先设置祖宗节点子孙节点也会得到该样式。
>
> 注意：并不是所有的样式都会被继承：

不可继承的：

```
display、margin、border、padding、background、height、min-height、max- height、width、min-width、max-width、overflow、position、left、right、top、 bottom、z-index、float、clear、table-layout、vertical-align、page-break-after、 page-bread-before和unicode-bidi
```

所有元素可继承：`visibility和cursor`

内联元素可继承：`letter-spacing、word-spacing、white-space、line-height、color、font、 font-family、font-size、font-style、font-variant、font-weight、text- decoration、text-transform、direction`

块状元素可继承：`text-indent和text-align`

列表元素可继承：`list-style、list-style-type、list-style-position、list-style-image`

表格元素可继承：`border-collapse`

## 单位

### size单位

`px`  pixel 像素,显示器的最小颗粒单位

相对单位

`em`   `  1em == 当前元素的font-size`

`rem`   `1em == 根元素（HTML）的font-size`

`%` 	父元素百分比

`vh`	viewport可视窗大小

`fr`	gird的特有单位（权重设置）

### color单位

`name`



词汇：**Hue**(色相)**saturation**(饱和度)**lightness**(亮度)**a**透明度(0-1) **0代表完全透明，1完全不透明**

**十六进制**缩写 `#000` 代表`#000000`			`#ff0`代表`#ffff00`

**百分比**0-255等价于0-100%

`rgb` 

`rgba`

`hsl`

`hsla`

## 常用属性

`list-style`:设置列表样式

`background-color`

`white-space`:对空白内容的处理方式 [MND](https://developer.mozilla.org/zh-CN/docs/Web/CSS/white-space)

`background-position`:对雪碧图进行切图的属性

### font-text-background

| 属性                                            | 值                                                           |
| ----------------------------------------------- | ------------------------------------------------------------ |
| `font-size`指定字体大小                         |                                                              |
| `font-family`                                   | 格式：serif 衬线字体sans-serif 非衬线字体monospace 等宽字体  |
|                                                 | 字体库：                                                     |
|                                                 | 字体形式**图标字体**、**emoji**…… [更多值](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-family) |
| @font-face定义字体族，见下方代码                | **慎用**：存在版权问题                                       |
| `iconfont`                                      | **单色图标**，通过**下载文件**或者CDN从外部网站引入          |
|                                                 | 常用字体图标 [阿里矢量图表库](https://www.iconfont.cn/help/detail?spm=a313x.7781069.1998910419.15&helptype=draw) [fontawesome](https://fontawesome.com/) |
| `font-weight` 指定字体粗细                      | [可选值](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-weight) |
| `font-style`                                    |                                                              |
| 属性简写                                        | font: 字体大小/行高 字体族 （行高 可以省略不写 如果不写使用默认值） |
| `line-height`                                   | 行间距=行高-字体大小                                         |
| `text-decoration`装饰                           |                                                              |
|                                                 | 默认值`none`  `underline` 下划线`line-through` 删除线`overline` 上划线 |
|                                                 | 可以设置**颜色**  **实线、虚线点、状虚线**等`border-style`也有的的属性 |
|                                                 |                                                              |
| `text-align` 文本的水平对齐方式                 | `left `左侧对齐`right `右对齐`center `居中对齐`justify `两端对齐 |
| `vertical-align`设置元素垂直对齐的方式          | `baseline` 默认值 基线对齐`top` 顶部对齐  `bottom `底部对齐`middle` 居中对齐 |
|                                                 | 基线对齐：类似于英语本的下方开始数的第二条线                 |
| `white-space`:对空白内容的处理方式              | `normal` 正常`nowrap` 不换行`pre` 保留空白                   |
| `text-overflow`对文本溢出内容的处理             | [可选值](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-overflow) |
| 背景                                            |                                                              |
| `background-image`引入                          | `url("xx");`                                                 |
| `background-repeat` 背景重复方式                | `repeat-x `**只**沿着x轴方向重复`repeat-y `**只**沿着y轴方向重复`no-repeat` 背景图片不重复 |
| `background-position`                           | [可选值](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-position) |
| `background-origin`设置背景的范围               |                                                              |
| `background-clip`设置背景的范围                 |                                                              |
| `background-attachment`背景图片是否跟随元素移动 | `scroll` 默认值 背景图片会跟随元素移动`fixed` 背景会固定在页面中，不会随元素移动 |
| `background-size`设置背景图片的大小             |                                                              |
|                                                 |                                                              |
|                                                 |                                                              |
|                                                 |                                                              |
|                                                 |                                                              |

```css
        @font-face {
                /* 指定字体的名字 */
            font-family:'myfont' ;
            /* 服务器中字体的路径 */
            src: url('./font/ZCOOLKuaiLe-Regular.ttf') format("truetype");
        }
        p{
        	font-family: myfont;//指定内置字体族
        }
```

### transition(移动)

