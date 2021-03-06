# \[typescript\]-env

## 简要介绍

> TypeScript是JavaScript的超集，以一门强数据类型的语言，适合**大型项目**开发？？？当然最终还是编译成JavaScript代码

## 环境配置

```bash
安装node.js（略）

$ npm install -g typescript #安装typescript的编译包

$ tsc -v # 查看版本

$ npm update -g typescript # 如果已经下载但是想要获取最新（稳定）版

$ tsc --init # 生成tsconfig.json文件
```

`tsconfig.json`[文件介绍](https://www.staging-typescript.org/tsconfig)

```javascript
{
  // 编译选项
  "compilerOptions": {
    // 输出目录
    "outDir": "./output",
    // 是否包含可以用于 debug 的 sourceMap
    "sourceMap": true,
    // 以严格模式解析
    "strict": true,
    // 采用的模块系统
    "module": "esnext",//commonjs
    // 如何处理模块
    "moduleResolution": "node",
    // 编译输出目标 ES 版本
    "target": "es5",
    // 允许从没有设置默认导出的模块中默认导入
    "allowSyntheticDefaultImports": true,
    // 将每个文件作为单独的模块
    "isolatedModules": false,
    // 启用装饰器
    "experimentalDecorators": true,
    // 启用设计类型元数据（用于反射）
    "emitDecoratorMetadata": true,
    // 在表达式和声明上有隐含的any类型时报错
    "noImplicitAny": false,
    // 不是函数的所有返回路径都有返回值时报错。
    "noImplicitReturns": true,
    // 从 tslib 导入外部帮助库: 比如__extends，__rest等
    "importHelpers": true,
    // 编译过程中打印文件名
    "listFiles": true,
    // 移除注释
    "removeComments": true,
    "suppressImplicitAnyIndexErrors": true,
    // 允许编译javascript文件
    "allowJs": true,
    // 解析非相对模块名的基准目录
    "baseUrl": "./",
    // 指定特殊模块的路径
    "paths": {
      "jquery": [
        "node_modules/jquery/dist/jquery"
      ],
    // 生成声明文件
    "declaration": true,
    // 监听文件变化执行编译
    "watch": true,
    },
    // typescript 语法检测支持的版本库，注意不是 polyfill！只是为了有对应版本的代码特性提示！
    "lib": [
      "es2015",
      "es2015.promise"
    ],
      "include": [
        // "syntax",// syntax下的全部
        // "syntax/*",// syntax下的一级目录
        // "syntax/*/*" // syntax下的二级目录
      ],
    "exclude": ["js", "node_modules"]
  }
}
```

编译命令：

```bash
$ tsc code.ts # 编译之后会出现同名的js代码`code.js`

$ tsc -p dirname # 使用该文件夹下的tsconfig.json -p 是--project的简写

$ node code.js
```

## `npm scirpt`

### `package.json`的脚本配置

语法：

通配符：`*`任意文件名，`**`任意文件名的子目录

`--`参数行

`npm run <command> [-- <args>]`

下面的示例为了方便演示，使用的简单命令，原谅我这种画蛇添足的用法

```javascript
"cli": "node"
```

```bash
npm run cli -- helloworld.js
```

`&`并行执行 `&&`顺序执行

```javascript
  "scripts": {
      // 实时编译
    "dev": "nodemon --ext ts --watch server --exec \"npm run clean && npm run build:ts && npm run server\"",
      //用于启动编译后的 node 服务器。
    "server": "cross-env NODE_ENV=development node ./output/app.js",
      //npm run clean 用于清空编译输出目录
    "clean": "rm -rf ./output.server",
     // npm run build:ts 用于编译 server 目录下的 TypeScript 文件
    "build:ts": "tsc -p ./server",
    "build": "npm run clean && npm run build:ts"
  }
```

## 简化编译和执行过程

两个方案

①`ts-node`包

```bash
$ npm i -g ts-node

$ ts-node code.ts # 编译+执行
```

缺点：不产生`js`文件,后续执行仍然需要花费编译时间，长代码体验极差

②vscode中的`tasks`

③封装`npm run`脚本

## 学习语法时的操作

```bash
配置中写入
  "exclude": ["node_modules"],
  "include": ["./src/**/*.ts"]

"scripts": {
"dev": "tsc --watch"
},

$ npm run dev

# 使用coderunner执行代码，如果是单入口项目可以使用nodemon实时监听变化
安装 ts-node 可以直接使用coderunner插件运行ts文件
```

