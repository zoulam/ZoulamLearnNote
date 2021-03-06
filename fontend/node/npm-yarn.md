---
description: 这里包含npmScript、以及包管理的注意事项，常用命令
---

# \[node\]npm yarn

## 1、npm

## 重要知识速记

```bash
双-是要大全拼 单-是简写
npm init -y 
| npm init --yes

npx
    node_modules/.bin/[moduleName] --version
    npx [moduleName] --version
    npx create-react-app <app-Name>使用完之后会删除包

npm -h
    可以查看命令

npm publish 之前需要注册并登录
npm adduser 注册（不建议，使用官方的图形化界面注册）
npm login 登录

在自己写的包下面npm link 在全局包目录下创建一个快捷方式
使用 npm link <packageName>

devDependencies(开发依赖，如打包工具、启动服务器的工具等)
dependencies（生产依赖，会被打包进static文件的依赖）

package.json
package-lock.json（有准确版本）
npm install的过程大致就是从package.json中读取所有的依赖信息，然后再与node_modules中已经安装的依赖进行对比，如果没有则通过package-lock.json获取相应版本号下载安装。如果已经存在则会通过package-lock.json检查更新。
如果出现错误
    在确定的自己包版本没问题的情况下删除package-lock.json再安装即可


npm list --depth  0 #查看当前目录下的包
npm list -g --depth  0 # 查看全局下安装的包
# --depth 0 是深度为0的一次，即不查看依赖的依赖.如果不填入就会一直递归

npm search <packageName> # 发布包之前检查是否存在同名的npm包
# 如果换源了需要注意换回来，不然会出现查询错误

查看可用版本
npm view <packageName>@* version
npm view vue@* version #查看vue的可用版本


npm update <packageName> # 更新指定包

npm outdated # 列举出过时包

# 查看字段
npm view <packageName> <field>

npm view <packageName> #查看package.json文件

---------------------------------下面列举常用的-----------------------------------
npm view <packageName> engines # 查看某个包的可用的最小版本node引擎
npm view express engines # { node: '>= 0.10.0' }


npm view <packageName> dependencies #子包依赖
npm view express dependencies

npm view <packageName> repository.url #包的npm仓库
npm view express repository.url

npm cache clean --force # 清除缓存
```

## 读取包的逻辑

当前目录下的 `node_modules`

没有就往上级目录找 `node_modules`

直到找到根路径，还是没有就使用全局的包

### 安装

**前言：npm是Node.js的依赖包管理工具，类似于JAVA的maven或者Gradle工具**

![&#x56FE;1](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200410100023699.png)

```text
npm -v
npm root -g
npm config list #查看配置信息
npm config set prefix "G:\npm-package\node_global"
npm config set cache "G:\npm-package\node_cache"
```

![&#x56FE;2](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200410101017646.png)

![&#x56FE;3](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200410100852815.png)

```bash
npm init               #对文件进行npm初始化(创建node_modules文件夹并新建json文件)，全部摁Enter键默认选项
npm init -y      #全部选yes

rm -rf node_modules/  #删除node_modules文件夹

npm install                    #局部安装package.json指定的依赖,默认安装最新（稳定版）,或者根据package.json信息安装
npm install packageName 0.0.1 # 安装该依赖的0.01版本

npm install -g vue        #在全局文件夹下安装vue依赖包

npm install vue            #安装vue依赖包，逻辑：先检查全局文件夹下是否存在vue文件，有则复制粘贴，无则下载

npm uninstall xx        #卸载指定依赖

npmn install cnpm           # 全局安装淘宝镜像命令

# 
npm install -g cnpm --registry=https://registry.npm.taobao.org

cnpm install xx            #用镜像网站安装xx，会出现json文件没有数据的情况

cnpm install xx --save      #安装xx包，并在package.json补全依赖信息
```

#### npm search

查找是否安装了指定的包，控制台会打印指定信息，也可以加上**-g**参数，查找全局下的包

### **换源**

**方式一**

```bash
npm config get registry  // 查看npm当前镜像源

npm config set registry https://registry.npm.taobao.org/  // 设置npm镜像源为淘宝镜像
npm config set https://registry.npmjs.org/

yarn config get registry  // 查看yarn当前镜像源

yarn config set registry https://registry.npm.taobao.org/  // 设置yarn镜像源为淘宝镜像

yarn config set registry  https://registry.yarnpkg.com // 老地址
```

**方式二**

```bash
npm install nrm -g
# 使用nrm工具切换淘宝源
npx nrm use taobao

# 如果之后需要切换回官方源可使用
npx nrm use npm

npm install -g yrm
yrm ls// 列举可用源
yrm use taobao //换源
yrm test taobao// 测速
```

### `"devDependencies"`和 `"dependencies"`

> `devDependencies`是开发依赖**（开发时）**
>
> 即开发工具存放依赖，不会打包进去
>
> 安装时必须填入参数 `-D` 或 `--save-dev`
>
> `npm i -D [packageName] [version]`
>
> `dependencies`生产依赖**（运行时）**
>
> 会被打包进去的依赖， **安装时不用填入参数也可** 即`--save` 或 `-S` 不填好像不会写入依赖信息

### 版本信息中的^和~

> 表示版本向上兼容的意思， `^` 的兼容：`4.x.x` ,`~`的兼容 `4.3.x`,其中的x标识可变动。

```bash
#这种方式安装就不会有这个符号，即锁定版本（exact是精确的意思）
npm install packagename --save-exact #或 -E
```

### npm script

> 作用是简化超长的命令

[阮一峰博客](http://www.ruanyifeng.com/blog/2016/10/npm_scripts.html)

[官方文档](https://docs.npmjs.com/misc/scripts) 这个我没看😪

#### 为什么可以直接使用node\_modules下的脚本

每次执行npm script都会创建一个环境变量（`node_modules/.bin`），然后再删除。

### npx

5.2之后的新命令

> 1、直接执行 `node_modules` 下的可执行指令
>
> 原来查看项目内的`nodemon`版本 `node_modulse/.bin/nodemon --version`
>
> 现在 `npx nodemon --version`
>
> 比较常用的是： `npx webpack`
>
> 2、安装的临时依赖**使用后就删除**（如：`npx create-react-app my-app`）

### `package.json`和 `package-lock.json`

> `npm install`的过程大致就是从`package.json`中读取所有的依赖信息，然后再与`node_modules`中已经安装的依赖进行对比，如果没有则通过`package-lock.json`获取相应版本号下载安装。如果已经存在则会通过`package-lock.json`检查更新。

`package.json`

初始化生成（仅记录用于安装的依赖信息）

`package-lock.json`

安装时生成，信息更详细（记录依赖的依赖信息）

### 依赖版本

**查看当前安装的版本信息**

`npm view [dependencieName] versions --json`

**从网络获取当前依赖的所有版本信息（避免打开网页的麻烦😛）**

`node_modules/.bin/[moduleName] --version`

npx命令简化为 `npx [moduleName] --version`

### 关于安装机制

[看这个吧](https://www.cnblogs.com/penghuwan/p/6970543.html)

## 2、yarn

### [安装](https://classic.yarnpkg.com/en/docs/install/#windows-stable)

```text
npm install -g yarn
```

### 常用命令

```bash
yarn init -y

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

### yarn install发生了什么？

```bash
[1/4] Resolving packages... # 解析包
[2/4] Fetching packages... # 下载包
[3/4] Linking dependencies... # 链接包
[4/4] Building fresh packages...# 构建依赖
```



### [版本中的符号信息](https://classic.yarnpkg.com/zh-Hans/docs/dependency-versions/)

### [more](http://yarnpkg.top/CLI.html)

> 直接磕头了，用得比较少，等要用上再总结

### workspaces

```json
  "private": true, // 不会发布到npm上
  "workspaces": [ // 对packages目录下的项目开启workspace模式
    "packages/*"
  ],
```

>  简单总结，当前 `packages`目录下存在项目 `project1`和 `project2`
>
> ​	1、`project1` 和 `project2`有共同的依赖，那么这个共同的依赖都会被安装到根目录下，而不是每一个子目录都安装
>
> ​	2、`project2`中依赖了 `project1`，但是 `project1`没有发布，传统方式是 `npm link` ，然后使用，但是 `workespace`能直接 import

## 3、nvm-windows

> 1、先卸载nodejs
>
> 2、安装指定版本的nodejs

[nvm-windows下载](https://github.com/coreybutler/nvm-windows/releases)

下载下面的免配置安装包

[nvm-setup.zip](https://github.com/coreybutler/nvm-windows/releases/download/1.1.7/nvm-setup)

卸载本地的node包（**注**：不卸载会出现切换失败的情况）

`控制面板` -> `找到node图标` -> `点击卸载`

```bash
nvm install 4.2.2 // 安装指定版本
nvm insatll 10.1 // 安装10.1.0的子版本
nvm insatll 10 // 安装10.0.0的子版本
nvm ls-remote // 查看可用版本【linux/mac】
nvm ls available // windows
nvm use 14.15.0 // 使用14.15.0版本的node
nvm alias lastest-version 14.15.0 // 起别名
nvm use lastest-version
nvm unalias lastest-version // 取消别名
nvm ls // 列出本地安装的所有版本
```

