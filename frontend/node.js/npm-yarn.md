---
description: 这里包含npmScript、以及包管理的注意事项，常用命令
---

# npm

## 安装

**前言：npm是Node.js的依赖包管理工具，类似于JAVA的maven或者Gradle工具**

![图1](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200410100023699.png)

```cmd
npm -v
npm root -g
npm config list #查看配置信息
npm config set prefix "F:\NODE_MODULE\node_global"
npm config set cache "F:\NODE_MODULE\node_cache"
```

![图2](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200410101017646.png)

![图3](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200410100852815.png)

```bash
npm init           	#对文件进行npm初始化(创建node_modules文件夹并新建json文件)，全部摁Enter键默认选项
npm init -y  	#全部选yes

rm -rf node_modules/  #删除node_modules文件夹

npm install           		 #局部安装package.json指定的依赖,默认安装最新（稳定版）,或者根据package.json信息安装
npm install packageName 0.0.1 # 安装该依赖的0.01版本

npm install -g vue		#在全局文件夹下安装vue依赖包

npm install vue			#安装vue依赖包，逻辑：先检查全局文件夹下是否存在vue文件，有则复制粘贴，无则下载

npm uninstall xx		#卸载指定依赖

npmn install cnpm           # 全局安装淘宝镜像命令

cnpm install xx			#用镜像网站安装xx，会出现json文件没有数据的情况

cnpm install xx --save	  #安装xx包，并在package.json补全依赖信息
```

###  `"devDependencies"`和 `"dependencies"`

> `devDependencies`是开发依赖**（开发时）**
>
> ​	即开发工具存放依赖，不会打包进去
>
> ​	安装时必须填入参数 `-D` 或 `--save-dev`
>
> ​	`npm i -D [packageName] [version]`
>
> `dependencies`生产依赖**（运行时）**
>
> ​	会被打包进去的依赖， **安装时不用填入参数也可**   即`--save` 或 `-S` 不填好像不会写入依赖信息

### 版本信息中的^

> 表示版本向上兼容的意思

```bash
#这种方式安装就不会有这个符号，即锁定版本（exact是精确的意思）
npm install packagename --save-exact #或 -E
```

## npm script

> 作用是简化超长的命令

[阮一峰博客](http://www.ruanyifeng.com/blog/2016/10/npm_scripts.html)

[官方文档](https://docs.npmjs.com/misc/scripts) 这个我没看😪

## npx

> 1、直接执行 `node_modules` 下的可执行指令
>
> 2、安装的临时依赖使用后就删除（如：`npx create-react-app my-app`）

## `package.json`和 `package-lock.json`

> ​	`npm install`的过程大致就是从`package.json`中读取所有的依赖信息，然后再与`node_modules`中已经安装的依赖进行对比，如果没有则通过`package-lock.json`获取相应版本号下载安装。如果已经存在则会通过`package-lock.json`检查更新。

`package.json` 

初始化生成（仅记录用于安装的依赖信息）

 `package-lock.json`

安装时生成，信息更详细（记录依赖的依赖信息）

## 依赖版本

**查看当前安装的版本信息**

`npm view [dependencieName] versions --json`

**从网络获取当前依赖的所有版本信息（避免打开网页的麻烦😛）**

`node_modules/.bin/[moduleName] --version`

npx命令简化为 `npx [moduleName] --version`

## 关于安装机制

[看这个吧](https://www.cnblogs.com/penghuwan/p/6970543.html)

# yarn

## [安装](https://classic.yarnpkg.com/en/docs/install/#windows-stable)

## 常用命令

```bash
yarn init

yarn add [package]
yarn add [package]@[version] # @version @1.2.3
yarn add [package]@[tag] # @tag可以是 @beta、@next或者@latest

# 添加到devDependencies，peerDependencies和optionalDependencies分别为：
yarn add [package] --dev
yarn add [package] --peer
yarn add [package] --optional

# 升级
yarn upgrade [package]
yarn upgrade [package]@[version]
yarn upgrade [package]@[tag]

# 删除
yarn remove [package]

#根据package.json 安装全部依赖
yarn 或 yarn install
```

## [版本中的符号信息](https://classic.yarnpkg.com/zh-Hans/docs/dependency-versions/)

## [more](http://yarnpkg.top/CLI.html)

> 直接磕头了，用得比较少，等要用上再总结