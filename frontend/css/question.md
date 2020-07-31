# 面试题

## 自行测试：

### 1.dsiplay：none；和visibility：hidden；的区别

前者是不占据页面空间隐藏，后者是设置为不可见，但是占据空间

### 2.修复高度塌陷问题

```css
.box1::after{/*选取造成高度塌陷后的标签，即使此element不存在*/
    content: '';
    display: block;/*将行元素转化为块元素*/
    clear: both;
}
```

### 3.外边距重叠问题

（子元素的外边距被父元素获取，干扰了父元素预期的位置。）

```css
box1::before{
      content: '';
      display: table;
    }
```

### 4.二三问题的综合解决问题（clearfix）

```css
box1::before,
box1::after{
    content: '';
    display: table;
    clear: both;
}
```

### 5.为什么a的伪元素一定要安装link→visited→hover→active的顺序书写

如果违反规则会让后面的伪类失效

### 6.box2居中于box1居中方式

将子元素设置为表格

```css
        .box1{
            width: 300px;
            height: 300px;
            background-color: orange;
            /* 将元素设置为单元格 td  */
            display: table-cell;
           vertical-align: middle;
        }
        
        .box2{
            width: 100px;
            height: 100px;
            background-color: yellow;
            margin: 0 auto;
        }
```

position

### 7.id和class的区别

id是唯一的，class一般用以代表一类标签（即可以不是唯一的），一般id（因为他的唯一性）是在JavaScript中使用DOM操作的。

### 8.import和link的区别[答案](https://www.cnblogs.com/my--sunshine/p/6872224.html)

**1.从属关系区别**
`@import`是 CSS 提供的语法规则，只有导入样式表的作用；`link`是HTML提供的标签，不仅可以加载 CSS 文件，还可以定义 RSS、rel 连接属性等。

**2.加载顺序区别**
加载页面时，`link`标签引入的 CSS 被同时加载；`@import`引入的 CSS 将在页面加载完毕后被加载。

**3.兼容性区别**
`@import`是 CSS2.1 才有的语法，故只可在 IE5+ 才能识别；`link`标签作为 HTML 元素，不存在兼容性问题。

**4.DOM可控性区别**
可以通过 JS 操作 DOM ，插入`link`标签来改变样式；由于 DOM 方法是基于文档的，无法使用`@import`的方式插入样式。

**5.权重区别(该项有争议，下文将详解)**
`link`引入的样式权重大于`@import`引入的样式。

### 9.选择器权重

!important > inline > id  > class or pseudo class >  element >  *  > inherit

继承指挥继承部分属性，这部分属性会干扰实际效果，需要自行重写。

　　==1、字体系列属性==

　　font-family：字体系列

　　font-weight：字体的粗细

　　font-size：字体的大小

　　font-style：字体的风格

　　==2、文本系列属性==

　　text-indent：文本缩进

　　text-align：文本水平对齐

　　line-height：行高

　　word-spacing：单词之间的间距

　　letter-spacing：中文或者字母之间的间距

　　text-transform：控制文本大小写（就是uppercase、lowercase、capitalize这三个）

　　color：文本颜色

　　==3、元素可见性：==

　　visibility：控制元素显示隐藏

　　==4、列表布局属性：==

　　list-style：列表风格，包括list-style-type、list-style-image等

　　==5、光标属性：==

　　cursor：光标显示为何种形态

### 10.伪类的使用场景和优点

不用为一些小的效果专门写一个类，省去部分JavaScript代码

### 11.什么是盒模型

HTML文档中的每个元素都被描绘成矩形盒子，这些矩形盒子通过一个模型来描述其占用空间，这个模型称为盒模型。盒模型通过四个边界来描述：margin（外边距），border（边框），padding（内边距），content（内容区域）。

内容区域content area 是包含元素真实内容的区域。它通常包含背景、颜色或者图片等，位于内容边界的内部，它的大小为内容宽度 或 content-box宽及内容高度或content-box高。

内边距区域padding area 延伸到包围padding的边框。如果内容区域content area设置了背景、颜色或者图片，这些样式将会延伸到padding上。它位于内边距边界内部, 它的大小为 padding-box 宽与 padding-box 高。

边框区域border area 是包含边框的区域，扩展了内边距区域。它位于边框边界内部，大小为 border-box 宽和 border-box 高。由 border-width 及简写属性 border控制。

外边距区域margin area用空白区域扩展边框区域，以分开相邻的元素。它的大小为 margin-box 的高宽。

如果答主能回答出来类似第一段描述，也说明他知道有关于CSS盒模型的概念。这个时候面试官可能会问出第二个有关于CSS盒模型的问题，比如CSS盒模型怎么计算？或许还会接着深入的问题，CSS盒模型有几种类型？他们之间怎么转换。如果这些你清楚，面试官或许还会回到最初的实用层面上来。比如：

有一个三列布局，页面的整体宽度是960px，左右两个侧边栏的宽度是20%，中间列的宽度是60%，中间列与两侧边栏的间距是20px。那么这样的一个三列布局如何实现？

### 12.自问自答

作者：peterwinners
链接：https://www.zhihu.com/question/333433404/answer/771595856
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。



1）如何登录鉴权。面试问这个时候我听过太多有趣的回答，比如有次一个人说登录就是后台返回true或者false，如果true就跳转，false就提示错误呗。然后我说既然是管理系统，有几个角色，那怎么区分他有那些页面和接口权限呢？她就不知道了。还有碰到过一次，那人说请求任何接口前都要让他再登录一次，我至今没想明白这个怎么操作。

2）和后台如何数据交互。比如项目接口是否遵循restful接口规范？常用的请求方法都有什么。我曾听到过一个面试者回答，所有接口都用get。我说那新增，删除，编辑也是get？？他回答是的。更不需要问http那些了。

3）用了UI框架那些组件。是不是觉得这个很幼稚，其实就是。因为我们体量小，肯定不可能自己造轮子。简历筛选完都是react+antd+dva的，然后问他的管理系统项目里用了antd什么组件，他用手比划了半天，说就是上面那个点了可以换图片的......持续半分钟后我突然意识到他想表达的是轮播。我试探性问他：你说的可是轮播？他高兴回答到：对，就是轮播。然后我问那个管理系统还用了别的什么呢？他说没有啊，就用了轮播。

4）表单输入校验。因为我们公司业务就是很多很多复杂表单。所以每回都想问问。前面说到简历筛选来的都是用react+antd+dva的。随便问问问用怎么写表单输入校验，怎么自己实现校验。印象深刻有次一个人说用onChange，用户填了放到state里，提交时候看有没有......我问你用的是antd吗？他说当然啊。

5）状态管理redux。亲爱的朋友们，看到这您是不是笑出了声，我们毕竟很小很现实。我也看过redux和react-redux源码，对于复杂点的业务当然需要啊。有次遇到一个人，我问他你项目里是否用了redux。他突然正襟危坐，我一愣神，他开始背诵:redux有三大设计原则,1..。我当时打断了他，就说说你项目里怎么用的吧。他并不理会我，坚持背诵。我等他背诵完了再问他，那到底用了没呢？他又背诵了一遍。

6）项目搭建。其实这个可以放到第一个说，碰到绝大部分人都说不是自己搭建的。那其实也可以接受，然后再问问那了不了解呢？大致目录结构什么的。碰到过一些人这个时候就说那些都不管，只写一个页面，集成有人做。记得碰到过一个人说他只写页面上的路由组件，我问什么那具体是什么，他说就在屏幕左边，点了就跳到别人页面上，跳哪个页面他也不管，一个项目他只写这个。我寻思这得多大体量公司，一个切换路由的菜单栏就安排一个人弄。还碰见过只写一个列表的，别的什么都不管。