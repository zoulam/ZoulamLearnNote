---
description: HTML一些常用的标签及属性介绍
---

# \[html\]入门

## 重要知识速记

```javascript
!DOCTYPE html
html lang
head
    meta
        charset
        name:keywords、description、viewport content
        http-equiv content
    title
link （href）
script defer="defer" async type="text/script" "module" src
style rel="stylesheet" href
body

src :script source img input(image) iframe
href:link a

hgroup header footer nav aside section article
 strong del select progress
source > audio source > video

ol>li
ul>li
dl>li

自闭 input img link meta br hr area ifame source

from(块) action entype method
input(行、自闭)  name value autocomplete=“off” readonly autofocus
                type:file text color checkbox(多选、依靠name分组) radio（单选）
                    button submit reset date random search email
          <button></button> 非自闭

 table: thead tbody tfoot
                 tr
                     td(colspan合并行、rowspan合并列)
```

## 主体结构：

```javascript
<!DOCTYPE html>
<html lang="zh">
    <head>
        <style></style>
    </head>
    <body></body>
    <script></script>
</html>
```

### 1、meta标签

```javascript
<!DOCTYPE html><!--告诉浏览器网页的版本-->
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <!--    - 编码
                - 将字符转换为二进制码的过程称为编码
            - 解码
                - 将二进制码转换为字符的过程称为解码
            - 字符集（charset）
                - 编码和解码所采用的规则称为字符集
            - 乱码问题：
                - 如果编码和解码所采用的字符集不同就会出现乱码问题
            - 常见的字符集：
                ASCII
                ISO88591
                GB2312
                GBK
                UTF-8，在开发时我们使用的字符集都是UTF-8    -->

    <!-- 除了定义编码方式，还可以定义关键字（搜索引擎搜索关键字）和描述（搜索引擎展示的超链接标题），这有利于提高搜索引擎的搜索成功，是提高seo的方式之一 -->
    <meta name="keywords" content="HTML5,前端,CSS3">
    <meta name="description" content="这是一个非常不错的网站">
<meta http-equiv="refresh" content="3;url=https://www.baidu.com"><!--网页重定向在打开连接后跳转百度，用途：域名过期或更换-->
    <meta name="viewport" content="width=device-width, initial-scale=1.0"><!--viewport:可视窗口大小设置-->
    <title></title><!--网页标题-->
    <style></style><!--内联样式-->
</head>
<body>
    <address>...</adderss><!--出现点击拨打电话等的效果-->
</body>
</html>
```

[address标签详解](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/address)

### 2、Scrpit标签

```javascript
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <!--  type="text/scrpit" 可省略不写，浏览器已经被JavaScript占领了-->
    <script type="text/scrpit" src="相对地址或CDN地址"></script>
    <!-- type="module" 模块化引入 -->
    <script src="xx" type="module"></script>
    <!--defer="defer" 延时加载  -->
    <script src="xx" defer="defer"></script>
    <!-- 异步加载 -->
    <script src="xx" async></script>
</head>
<body>
</body>
</html>
```

### 3、常见seo优化方式

* 合理的**title**、**description**、**keywords**：搜索对着三项的权重逐个减小，title值强调重点即可，重要关键词出现不要超过2次，而且要靠前，不同页面title要有所不同；description把页面内容高度概括，长度合适，不可过分堆砌关键词，不同页面description有所不同；keywords列举出重要关键词即可
* **语义化**的HTML代码，符合W3C规范：语义化代码让搜索引擎容易理解网页
* 重要内容HTML代码放在最前：搜索引擎抓取HTML顺序是从上到下，有的搜索引擎对抓取长度有限制，保证重要内容一定会被抓取
* 重要内容不要用js输出：爬虫不会执行js获取内容
* 少用**iframe**：搜索引擎不会抓取iframe中的内容
* 非装饰性图片必须加**alt**
* 提高网站速度：网站速度是搜索引擎排序的一个重要指标
* **4、其他语义化标签**

```javascript
     <!-- 
        header 表示网页的头部
        main 表示网页的主体部分(一个页面中只会有一个main)
        footer 表示网页的底部加入id或者class之后用作快速跳转虽然程        序员都知道end键但用户就是不知道
        nav 表示网页中的导航
        aside 和主体相关的其他内容（侧边栏）
        article 表示一个独立的文章
        section 表示一个独立的区块，上边的标签都不能表示时使用            section
        div 没有语义，就用来表示一个区块，目前来讲div还是我们主要的         布局元素
        span 行内元素，没有任何的语义，一般用于在网页中选中文字
      -->
     <header></header>
     <main></main>
     <footer></footer>
     <nav></nav>
     <aside></aside>
     <article></article>
     <section></section>
     <div></div>
     <span></span>
```

### 5、实体

> HTML的标签占用了部分符号，所以使用实体来表示被占用的内容

```javascript
    <p> &nbsp;</p><!-- 空格 -->
    <p> &gt;</p><!-- 大于 -->
    <p> &lt;</p><!-- 小于 -->
    <p> &copy;</p><!-- 版权 -->
```

[常用实体表](https://www.w3school.com.cn/html/html_entities.asp)

[所有的实体](https://html.spec.whatwg.org/multipage/named-characters.html#named-character-references)

### 6、万能的div和span

优点：div和span标签可以帮助解决所有的问题，减少记忆成本。

缺点：与浏览器读取方式不契合，即无语义化，对seo不利。

### 7、块元素、行元素

**常用块元素**

```javascript
<!--                
        - 块元素
            - 块元素会在页面中独占一行(自上向下垂直排列)
            - 默认宽度是父元素的全部（会把父元素撑满）
            - 默认高度是被内容撑开（子元素）
-->
    <group>...</group><!--包裹大量标题h1、h2……  hgroup标签用来为标题分组，可以将一组相关的标题同时放入到hgroup-->
        <h1>...</h1> <!--标题一级  -->
        <hr> <!-- 水平分割线 -->
        <p>...</p> <!-- 段落 --> paragraph 
        <pre>...</pre> <!-- 预格式化 -->
        <blockquote>...</blockquote><!--  段落缩进 前后5个字符 -->
        <marquee>...</marquee> <!-- 滚动文本 -->
        <ul>...</ul> <!-- 无序列表 -->
        <ol>...</ol> <!-- 有序列表 -->
        <dl>...</dl><!--  定义列表 -->
        <table>...</table> <!-- 表格 -->
        <form>...</form> <!-- 表单 -->
        <div>...</div>
    <iframe src="https://www.qq.com" width="800" height="600" frameborder="0"></iframe>
    <!--
        内联框架，用于向当前页面中引入一个其他页面，不利于爬虫和seo
            src 指定要引入的网页的路径
            frameborder 指定内联框架的边框大小
     -->
```

**常用行元素**

**在使用display:inline-block；之后可以实现行内块元素**

```javascript
<!--    - 行内元素
            - 行内元素不会独占页面的一行，只占自身的大小
            - 行内元素在页面中左向右水平排列，如果一行之中不能容纳下所有的行内元素则元素会换到第二行继续自左向右排列（书写习惯一致）
            - 行内元素的默认宽度和高度都是被内容撑开
            - 行内元素只能行内元素，不能包含块级元素 

-->
        <span>...</span>
        <a target="_blank">...</a>
<!--  链接 用于跳转其他页面也可以用作跳转某个id标签，也就是定位到网页的某个具体位置，其中target是归档在什么地方打开超链接，_blank:在空白页打开-->
<a href="javascript:;">这是一个新的超链接</a>
<!--这是一个无实际内容的超链接-->
        <br>  <!-- 换行 -->
        <b>...</b>  <!-- 加粗 -->
        <strong>...</strong>  <!-- 加粗 -->
        <img/>  <!-- 图片 -->
        <sup>...</sup>  <!-- 上标 -->
        <sub>...</sub>  <!-- 下标 -->
        <i>...</i>  <!-- 斜体 -->
        <em>...</em>  <!-- 斜体 -->
        <del>...</del>  <!-- 删除线 -->
        <u>...</u>  <!-- 下划线 -->
        <input>...</input>  <!-- 文本框 -->
        <textarea>...</textarea>  <!-- 多行文本 -->
        <select>...</select>  <!-- 下拉列表 -->
<blockquote></blockquote><!-- blockquote 表示一个长引用 -->
      子曰<q>学而时习之，乐呵乐呵！</q><!--q表示一个短引用效果会给q标签内容加上双引号 -->
```

**自闭标签**

**HTML5中不用写/**

```JavaScript
<img>       或      <img/>
<input>     或      <input/>
<link>
<img>
<br>
<hr> 结束符号
<area>
<source>
```

### 8、音频视频图片

**出现兼容性问题，如IE无法加载内容,可以用embed来支持，可以调用系统本身的播放器**

**图片**



```javascript
 <img src="./girl.jpg" alt="女孩" title="girl"> <!-- alt是图片失效后显示，title是hover时显示  -->
```

**路径**

../跳至上级目录

./同级目录下，可省略

url:网络图片地址

**图片文件格式类型：**

> **jpeg\(jpg\)**
>
> * 支持的颜色比较丰富，不支持透明效果，不支持动图
> * 一般用来显示照片
>
> **gif**
>
> * 支持的颜色比较少，支持简单透明，支持动图
> * 颜色单一的图片，动图
>
> **png**
>
> * 支持的颜色丰富，支持复杂透明，不支持动图
> * 颜色丰富，复杂透明图片（专为网页而生）
>
> **webp**
>
> * 这种格式是谷歌新推出的专门用来表示网页中的图片的一种格式
> * 它具备其他图片格式的所有优点，而且文件还特别的小（有细微的图片质量损失）
> * 缺点：兼容性不好
>
> **base64**\([base64介绍](https://zh.wikipedia.org/zh-hans/Base64)\)
>
> * 将图片使用base64编码，这样可以将图片转换为字符，通过字符的形式来引入图片
> * 一般都是一些需要和网页一起加载的图片才会使用base64
>
>   效果一样，用小的
>
>   效果不一样，用效果好的

**音频视频**

```JavaScript
    <!-- 
        audio 标签用来向页面中引入一个外部的音频文件的
            音视频文件引入时，默认情况下不允许用户自己控制播放停止

        属性：
            controls 是否允许用户控制播放
            autoplay 音频文件是否自动播放
                - 如果设置了autoplay 则音乐在打开页面时会自动播放
                    但是目前来讲大部分浏览器都不会自动对音乐进行播放 
            loop 音乐是否循环播放  
     -->
    <!-- <audio src="./source/audio.mp3" controls autoplay loop></audio> -->

    <!-- <audio src="./source/audio.mp3" controls></audio> -->


    <!-- 除了通过src来指定外部文件的路径以外，还可以通过source来指定文件的路径 -->
    <audio controls>
        <!-- 对不起，您的浏览器不支持播放音频！请升级浏览器！ -->
        <source src="./source/audio.mp3">
        <source src="./source/audio.ogg">
        <embed src="./source/audio.mp3" type="audio/mp3" width="300" height="100">
    </audio>

    <!-- 
        使用video标签来向网页中引入一个视频
            - 使用方式和audio基本上是一样的
     -->
    <video controls>
        <source src="./source/flower.webm">
        <source src="./source/flower.mp4">
        <embed src="./source/flower.mp4" type="video/mp4">
    </video>
```

**音频视频文件格式：**

MP3、ogg：ogg的文件较小小、文件大小相同时码率高，但会有兼容性问题

mp4、webm：mp4较为清晰但文件大、webm文件小且兼容性有问题

### 9、列表

| 名称 | 语义 |
| :--- | :--- |
| `li`**（list）** | 包裹在ol或ul内的标签 |
| `ol`**order list** | 有序列表\(前面会有序号\) |
| `ul`**unorder list** | 无序列表\(默认前面是黑色圆点\) |
| `dl` | 术语列表描述 |
| `dt` | 术语名\(key\) |
| `dd` | 术语介绍\(value\) |

```javascript
<body>
    <!--
    列表（list）
        1、铅笔
        2、尺子
        3、橡皮

    在html中也可以创建列表，html列表一共有三种：
        1、有序列表
        2、无序列表
        3、定义列表

    无序列表，使用ol标签来创建无序列表
        使用li表示列表项

    无序列表，使用ul标签来创建无序列表
        使用li表示列表项

    定义列表，使用dl标签来创建一个定义列表
        使用dt来表示定义的内容
        使用dd来对内容进行解释说明

    列表之间可以互相嵌套

 -->

    <ul>
        <li>结构</li>
        <li>表现</li>
        <li>行为</li>
    </ul>

    <ol>
        <li>结构</li>
        <li>表现</li>
        <li>行为</li>
    </ol>


    <dl>
        <dt>结构</dt>
        <dd>结构表示网页的结构，结构用来规定网页中哪里是标题，哪里是段落</dd>
        <dd>结构表示网页的结构，结构用来规定网页中哪里是标题，哪里是段落</dd>
        <dd>结构表示网页的结构，结构用来规定网页中哪里是标题，哪里是段落</dd>
    </dl>

    <ul>
        <li>
            aa
            <ul>
                <li>aa-1</li>
                <li>aa-2
                    <ul>
                        <li>aa-1</li>
                        <li>aa-2</li>
                    </ul>
                </li>
            </ul>
        </li>
    </ul>


</body>

</html>
```

### 10、表格\(table\)

> table也可以用作布局，但实际场景中禁止使用

```javascript
<!--
        在现实生活中，我们经常需要使用表格来表示一些格式化数据：
            课程表、人名单、成绩单....

        同样在网页中我们也需要使用表格，我们通过table标签来创建一个表格

 -->
<table border="1" width='50%' align="center">
        <!-- 在table中使用tr表示表格中的一行，有几个tr就有几行 -->
        <tr>
            <!-- 在tr中使用td表示一个单元格，有几个td就有几个单元格 -->
            <td>A1</td>
            <td>B1</td>
            <td>C1</td>
            <td>D1</td>
        </tr>
        <tr>
            <td>A2</td>
            <td>B2</td>
            <td>C2</td>
            <!-- rowspan 纵向的合并单元格 -->
            <td rowspan="2">D2</td>
        </tr>
        <tr>
            <td>A3</td>
            <td>B3</td>
            <td>C3</td>
        </tr>
        <tr>
            <td>A4</td>
            <td>B4</td>
            <!--
                colspan 横向的合并单元格
             -->
            <td colspan="2">C4</td>
        </tr>
    </table>
```

**长表格**

表格长度无法确定

```JavaScript
<body>
    <table border="1" width='50%' align="center">
        <!--
            可以将一个表格分成三个部分：
                头部 thead
                主体 tbody
                底部 tfoot

                th 表示头部的单元格
         -->
        <thead>
            <tr>
                <th>日期</th>
                <th>收入</th>
                <th>支出</th>
                <th>合计</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>2000.1.1</td>
                <td>500</td>
                <td>200</td>
                <td>300</td>
            </tr>
            <tr>
                <td>2000.1.1</td>
                <td>500</td>
                <td>200</td>
                <td>300</td>
            </tr>
            <tr>
                <td>2000.1.1</td>
                <td>500</td>
                <td>200</td>
                <td>300</td>
            </tr>
            <tr>
                <td>2000.1.1</td>
                <td>500</td>
                <td>200</td>
                <td>300</td>
            </tr>
        </tbody>
        <tfoot>
            <tr>
                <td></td>
                <td></td>
                <td>合计</td>
                <td>300</td>
            </tr>
        </tfoot>
    </table>
</body>
```

### Tips：

**如果表格中没有使用tbody而是直接使用tr，**

**那么浏览器会自动创建一个tbody，并且将tr全都放到tbody中**

**tr不是table的子元素**

### 11、表单（form）

**input的常用type**

| `<input type="">` | 效果 |
| :--- | :--- |
| `button`\(button有单独的标签、里面也有`type="button"`、`type="reset"`、`type="submit"`\) | 按钮 |
| `reset` | 重置表单 |
| `submit` | 提交表单 |
| `text` | 单行文本输入框 |
| `color` | 取色器 |
| `file` | 文件上传 |
| `password` | 密码 |
| `radio` | 圆形单选框 |
| `checkbox` | 多选框或todolist框 |
| `random` | 可拖动控件\(类似windows的音量调节键\) |
| `search` | 查找框 |
| `email` | 邮箱格式 |

[更多type](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/Input)

```javascript
<form action="服务器地址">

    <!--文本框-->
   <input type="text" name="username">
    <!--
        此处提交后若填入zoulam提交成功后url会变成 服务器地址?    username=zoulam
-->

    <!--单选框、name是相同的、checked是默认选项-->    
    <input type="radio" name="hello" value="a">
        <input type="radio" name="hello" value="b"checked>

            <!--多选框-->
 <input type="checkbox" name="test" value="1">
        <input type="checkbox" name="test" value="2">
        <input type="checkbox" name="test" value="3" checked>

            <!-- 下拉框 、selected设置默认选项-->
        <select name="haha">
            <option value="i">选项一</option>
            <option selected value="ii">选项二</option>
            <option value="iii">选项三</option>
        </select>

    <!--表单提交按钮、触发form的action-->
    <input type="submit" value="注册">
    </form>
```

**input的常用属性**

| 属性 | 效果 |
| :--- | :--- |
| `value="123456"` | 在可填入值的位置填入默认可删除的`123456` |
| `name="单选1"` | 对表单进行分组，配合单选多选等使用 |
| `autocomplete="off"` | 关闭自动补全 默认是打开 |
| `readonly` | 将表单项设置为只读，数据会提交 |
| `disabled` | 将表单项设置为禁用，数据不会提交 |
| `autofocus` | 设置表单项自动获取焦点 |

```javascript
<body>
    <form action="target.html">
<!--
    autocomplete="off" 关闭自动补全 默认是打开
    readonly 将表单项设置为只读，数据会提交
    disabled 将表单项设置为禁用，数据不会提交
    autofocus 设置表单项自动获取焦点
 -->
        <input type="text" name="username" value="hello" readonly>
        <br><br>
        <input type="text" name="username" autofocus autocomplete="off">
        <br><br>
        <input type="text" name="b" value="123" disabled>
        <br><br>

        <!-- <input type="color"> -->
        <br><br>
        <!-- <input type="email"> -->
        <br><br>
        <input type="submit">

        <!-- 重置按钮 -->
        <input type="reset">

        <!-- 普通的按钮 -->
        <input type="button" value="按钮">
        <br><br>
        <button type="submit">提交</button>
        <button type="reset">重置</button>
        <button type="button">按钮</button>
    </form>
</body>
```

### 12、快捷输入

> 注：此快捷方式是在vscode上的，其他IDE我不确定能不能用，不过有些逻辑太复杂又不常用的我还是懒得记。

```javascript
<!--   ! ：是设置默认HTML模板  -->


<!-- 父子孙：div>nav>p -->
    <div>
        <nav>
            <p></p>
        </nav>
    </div>

    <!-- 父子(兄弟)：div>p+a -->
    <div>
        <p></p>
        <a href=""></a>
    </div>

    <!-- 一爷一父多子孙：div>nav>p*2 -->
    <div>
        <nav>
            <p></p>
            <p></p>
        </nav>
    </div>

    <!-- 一父亲多组子孙：div>(nav>p)*2 -->
    <div>
        <nav>
            <p></p>
        </nav>
        <nav>
            <p></p>
        </nav>
    </div>

    <!-- 包含文本信息：div>p{test} -->
    <div>
        <p>test</p>
    </div>

    <!-- 顺序递增内容：div{$}*5 -->
    <div>1</div>
    <div>2</div>
    <div>3</div>
    <div>4</div>
    <div>5</div>

    <!-- 顺序递增内容(前面的$是占位符)：div{$$}*5 -->
    <div>01</div>
    <div>02</div>
    <div>03</div>
    <div>04</div>
    <div>05</div>

    <!-- 从非1的数字开始递增（此处从6开始）： div{$$@6}*3-->
    <div>06</div>
    <div>07</div>
    <div>08</div>

    <!-- 填入id内容：#5-->
    <div id="5"></div>

    <!-- 填入class内容：.table -->
    <div class="table"></div>

    <!-- 非div快速定义： ol.a-->
    <ol class="a"></ol>

    <!-- 多种属性： a[href=dd id=$ class=table]*2 -->
    <a href="dd" id="1" class="table"></a>
    <a href="dd" id="2" class="table"></a>

    <!-- 案例使用:    .nav>(.item>img[src=$.png]+p{菜单$})*2    -->
    <div class="nav">
        <div class="item">
            <img src="1.png" alt="">
            <p>菜单1</p>
        </div>
        <div class="item">
            <img src="2.png" alt="">
            <p>菜单2</p>
        </div>
    </div>
    <!--生成测试文本：lorem-->
    Lorem ipsum dolor sit amet consectetur adipisicing elit. Mollitia est debitis repellat? Eveniet, vitae et nobis consequuntur sit rem repellat, minima nulla recusandae ratione quisquam ex quae ab dignissimos. Ducimus.
```

本文仅仅介绍了HTML的常用内容，更多详细内容请访问[MDN文档](https://developer.mozilla.org/zh-CN/)

