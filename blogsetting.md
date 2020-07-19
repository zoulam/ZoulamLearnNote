---
description: "其实主要是图床搭建啦\U0001F613"
---

# 博客简要配置

> 图床：PicGo+阿里云对象存储OSS服务
>
> 本地markdown编辑器：typora
>
> 博客直接用gitbook
>
> **PicGo简要介绍**：使用Electron+Vue开发的图床上传和获取Url的软件。

## 图床配置：

![typora&#x7684;&#x56FE;&#x7247;&#x914D;&#x7F6E;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200716173601669.png)

## 阿里云OSS配置

### 注册账号并实名认证\[略\]

### 购买服务

![bucket&#x521B;&#x5EFA;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200716174859140.png)

**填写Bucket名称，其余按照个人需要选取配置，当然也可以是默认**

### 获取配置项

```javascript
{
  "picBed": {
    "uploader": "aliyun",
    "aliyun": {
     // 必填
    "accessKeyId": "",
    "accessKeySecret": "",
    "bucket": "", 
    "area": "", 
    "customUrl": "", 
     // 可不修改
    "path": "img/", //在你的仓库下面创建文件夹存储，方便查找
     "options": "" // 针对图片的一些后缀处理参数 PicGo 2.2.0+ PicGo-Core 1.4.0+
    }
  },
  "picgoPlugins": {}
}
```

#### 获取accessKeyId、accessKeySecret

**accessKey创建并获取**

![accessKey&#x5165;&#x53E3;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200716175028051.png)

![](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200716175421637.png)

#### 获取bucket、area、path

![&#x914D;&#x7F6E;&#x9879;&#x83B7;&#x53D6;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200716174531342.png)

### 填入config.json并测试

![&#x914D;&#x7F6E;&#x6D4B;&#x8BD5;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200716175726772.png)

我的测试失败了，但是仍然能够上传成功，所以不用太担心

![&#x6D4B;&#x8BD5;&#x7ED3;&#x679C;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200716175811610.png)

## GitBook使用

最后就是注册GitBook账号创建自己的Space了。

**优点：**

①选择GitBook主要是因为个人用户可以免费，而且UI看起来是最干净的。

②可以跟GitHub仓库同步，也就是说可以用git管理版本，很符合平时git的使用习惯。

**缺点：**

~~①当然免费用户不能**多人协作**和**使用个性化主题**；~~

②还有一个挺致命的问题，就是国内用户打开太慢了。

> 不过总结来说，上面的的缺点主要是因为没钱。

### 个性化

> 发布在自己的域名内才需要做这些事情，githubio页面不需要

可以安装个性化插件，美化和添加gitbook功能。

gitbook自带以下五个插件，安装同类型插件时需要先卸载。

> highlight： 代码高亮
> search： 导航栏查询功能
> sharing：右上角分享功能
> font-settings：字体设置（最上方的"A"符号）
> livereload：为GitBook实时重新加载

#### 插件安装

在`book.json`文件下写入以下内容

prism：代码highlight

search-pro：查找（支持中文）

back-to-top-button：返回顶部按钮

page-treeview：页内目录树

```json
{
  "plugins": [
    "prism",
    "-highlight",
    "-lunr",
    "-search",
    "search-pro",
    "back-to-top-button",
    "page-treeview"
  ],
  "pluginsConfig": {
    "page-treeview": {
      "minHeaderCount": "2",
      "minHeaderDeep": "2"
    },
    "prism": {
      "css": ["prismjs/themes/prism-okaidia.css"],
      "lang": {
        "flow": "typescript"
      }
    }
  }
}

```

全局安装脚手架`npm install gitbook-cli -g`

通过查询版本安装gitbook：`gitbook --version`

gitbook安装插件：`gitbook install` 缺陷：每一个都重新安装，比较慢

gitbook启动服务：`gitbook serve`

​				会生成`_book`文件，也就是最终的页面文件

gitbook打包:`gitbook build`



这里就默认你安装了**node.js**了，且带有新版的npm，就可以实现指定插件安装

再使用安装命令`npm i gitbook-plugin-prism `

#### gitbook命令大全

```bash
build [book] [output]       build a book
        --log                   Minimum log level to display (Default is info; Values are debug, info, warn, error, disabled)
        --format                Format to build to (Default is website; Values are website, json, ebook)
        --[no-]timing           Print timing debug information (Default is false)

serve [book] [output]       serve the book as a website for testing
    --port                  Port for server to listen on (Default is 4000)
    --lrport                Port for livereload server to listen on (Default is 35729)
    --[no-]watch            Enable file watcher and live reloading (Default is true)
    --[no-]live             Enable live reloading (Default is true)
    --[no-]open             Enable opening book in browser (Default is false)
    --browser               Specify browser for opening book (Default is )
    --log                   Minimum log level to display (Default is info; Values are debug, info, warn, error, disabled)
    --format                Format to build to (Default is website; Values are website, json, ebook)

install [book]              install all plugins dependencies
    --log                   Minimum log level to display (Default is info; Values are debug, info, warn, error, disabled)

parse [book]                parse and print debug information about a book
    --log                   Minimum log level to display (Default is info; Values are debug, info, warn, error, disabled)

init [book]                 setup and create files for chapters
    --log                   Minimum log level to display (Default is info; Values are debug, info, warn, error, disabled)

pdf [book] [output]         build a book into an ebook file
    --log                   Minimum log level to display (Default is info; Values are debug, info, warn, error, disabled)

epub [book] [output]        build a book into an ebook file
    --log                   Minimum log level to display (Default is info; Values are debug, info, warn, error, disabled)

mobi [book] [output]        build a book into an ebook file
    --log                   Minimum log level to display (Default is info; Values are debug, info, warn, error, disabled)
```

# 常见错误

## 1）Error: ENOENT: no such file or directory, stat'xx'

修改`C:\Users\Administrator\.gitbook\versions\3.2.3\lib\output\website\copyPluginAssets.js`下的文件

将112行的配置`confirm: true`修改为`confirm: false`

```javascript
    return fs.copyDir(
        assetsFolder,
        assetOutputFolder,
        {
            deleteFirst: false,
            overwrite: true,
            confirm: false
        }
    );
```

