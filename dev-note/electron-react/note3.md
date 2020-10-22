# note3

# 1、打包

## ①前置知识

### 工具

[electron打包](https://www.electronjs.org/docs/tutorial/application-packaging)

[electron-builder](https://github.com/electron-userland/electron-builder)

​	[配置](https://www.electron.build/configuration/configuration)

```json
    "pack": "electron-builder --dir",#已安装
    "dist": "electron-builder",# 打包出安装包
```

[react性能优化](https://zh-hans.reactjs.org/docs/optimizing-performance.html)

### 开发环境和生产环境

开发环境：

​	1、允许错误和调试

​	2、可以输出丰富的信息

​	3、mock数据

​	4、devDependencies

生产环境：

​	1、访问快

​	2、无错误

​	3、真实数据

​	4、dependencies

## ②开始打包

`npm run build` 脚手架配备的命令

**npm script**钩子脚本： `"preXX"` 在xx命令执行前执行`"postXX"`在xx命令执行后执行

# 2、builder后的文件

![主体结构](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201008222603753.png)

[asar文件](https://fileinfo.com/extension/asar)

可以使用 `npm i -g asar`进行解压缩查看代码

```bash
asar extract app.asar ./app
#asar extract <filename> <dir>
```

解压缩之后就是外面的源码

[npm-asar](https://www.npmjs.com/package/asar)

## ①小优化

缩小 `node_modules`

electron的mian.js优化：使用webpack进行优化：通过入口寻找依赖打包，而不是逐个添加

```json
    "prerelease": "npm run build && npm run buildMain",
    # npm run build 打包react # npm run buildMain 打包electron
```

# 3、自动更新

## ①发布

[publish](https://www.electron.build/configuration/publish.html)

[release](https://www.electron.build/configuration/publish.html#how-to-publish)

release 除了脚本还有发布功能

```
    "release": "electron-builder",
    "prerelease": "npm run build && npm run buildMain",
```

创建GitHub仓库

[GH_TOKEN](https://www.electron.build/configuration/publish.html)

记得将token权限打开

![允许提交到公开的仓库](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201009213520942.png)

```powershell
[Environment]::SetEnvironmentVariable("GH_TOKEN","<YOUR_TOKEN_HERE>","User")
[Environment]::SetEnvironmentVariable("GH_TOKEN","779590896347f3f94bf7cd30fd85b5a7bea6fadb","User")
```

## ②更新

[electron-updates](https://www.electronjs.org/docs/tutorial/updates)

[electron-builder更新](https://www.electron.build/auto-update.html)

```
npm install electron-updater --save-dev
```

[electron-updater事件](https://www.electron.build/auto-update.html#events)

[更新的代码模板](https://github.com/iffy/electron-updater-example)

发布之后是`Draft`（草案）阶段，需要添加description和Publish，才能被用户获取

## ③配置开发环境调试自动更新

mock文件

if(isDev)读取自己写的配置文件