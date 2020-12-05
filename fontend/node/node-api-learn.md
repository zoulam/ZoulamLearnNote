# \[node\]api-learn

>  收录常用`api`，具体自己去看官网

## [各种log](https://developer.mozilla.org/zh-CN/docs/Web/API/console)

> 只有`process.stdout.write`是node特有的

```javascript
// 断言、判断内容真假
console.assert(0);//Assertion failed
console.assert(1);// 无输出内容

// 日志打印，换行
console.log(222);

// 清空控制台
// console.clear();//浏览器中输出：Console was cleared

// 进程的打印方法(node)
process.stdout.write(`我不换行`);
process.stdout.write(`我真的不换行\n`);

// 打印错误信息(node环境没有红色)
console.error('我是一条红色的错误信息！');

// 打印bug信息（对象形式，后续的对象存储在数组中）
console.debug(
    {
        error: 404,
        msg: "server error"
    },
    [{
        error: 500,
        msg: "reload"
    }, {
        error: 200,
        msg: "connect sucess"
    }]
);

//打印通知消息，没看出和log有什么区别
console.info(
    {
        error: 404,
        msg: "server error"
    },
    [{
        error: 500,
        msg: "reload"
    }, {
        error: 200,
        msg: "connect sucess"
    }]
);

//以表格的形式打印(数组index)
console.table(["apples", "oranges", "bananas"]);
console.table({
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaamother: "apples",
    father: "oranges",
    me: "bananas"
});

// 内置计时器，精确到ms，即秒的小数点后3位
console.time();
setTimeout(() => {
    console.timeEnd(); // 1.002s
}, 1000)
```

<img src="https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201203224005636.png" alt="20201203224005636" style="zoom:67%;" />

## path

| 方法                                | 变量         | 描述                           |
| ----------------------------------- | ------------ | ------------------------------ |
|                                     | `__filename` |                                |
|                                     | `__dirname`  |                                |
| `basename(path, ignoreContent)`     |              |                                |
| `__filename = basename()+dirname()` |              |                                |
| `extname()`                         |              | 获取扩展名                     |
| `join`                              |              | 格式化路径                     |
| `resolve`                           |              | 拼接绝对路径，即会补上路径头部 |



```javascript
const path = require('path')
// 两个变量
console.log(__dirname) // js文件路径
// c:\Users\zoulam\Desktop\node-operation\01path-file
console.log(__filename)// js文件名
// c:\Users\zoulam\Desktop\node-operation\01path-file\00path.js

console.log(path.basename('C:\\temp\\myfile.html'))// 文件名（带后缀）
// myfile.html
console.log(path.basename('C:\\temp\\myfile.html','e.html'))// 文件名忽略第二个参数内容
// myfil
console.log(path.dirname('/目录1/目录2/目录3/app.js')) // 和basename互补获取文件路径
// 目录1/目录2/目录3

// 读取环境变量（如你配置的jdk环境变量）
console.log(process.env.PATH)
// 分割环境变量 delimiter
console.log(process.env.PATH.split(path.delimiter))// 数组，以不同的环境插入不同的杠
// [环境变量1, 环境变量2, ……]


console.log(path.extname('index.html'))// 获取扩展名
// .html

// 拼接绝对路径需要加上__dirname
console.log(path.join('/目录1', '目录2', '目录3/目录4', '目录5', '..'))
// \目录1\目录2\目录3\目录4
// 自动补/ 和操作系统的cd命令类似 ..返回上级目录

// 从右往左解析
console.log(path.resolve('目录1', '目录2/目录3/', '../目录4/文件.gif'))// 头部都没有 /全部解析
// C:\Users\zoulam\Desktop\node-operation\目录1\目录2\目录4\文件.gif
console.log(path.resolve('目录1'))
// C:\Users\zoulam\Desktop\node-operation\目录1
console.log(path.resolve('/目录1', '/目录2', '目录3'))// 目录二头部有杠到此终止
// C:\目录2\目录3
console.log(path.resolve('/目录1/目录2', '/目录3/目录4/'))// 目录三头部有杠终止，并且格式化，去除尾部的杠
// C:\目录3\目录4
```

## fs

| 方法           | 描述                                     |
| -------------- | ---------------------------------------- |
| `unlinkSync()` |                                          |
| `unlink()`     |                                          |
| `readFile()`   | 读取的数据是二进制的，`toString()`就好了 |
|                |                                          |
|                |                                          |
|                |                                          |
|                |                                          |
|                |                                          |
|                |                                          |
|                |                                          |



>  下面以文件的增删改查为例，单个操作不需要异步编程，可以直接使用同步操作，但是基本不存在单操作

```javascript
const fs = require('fs');
const path = require('path')
let filePath = path.resolve(__dirname, 'test.md')
try {
    fs.unlinkSync(filePath);
    console.log('已成功删除文件');
} catch (err) {
    console.log('删除失败')
}
```

### cb

```javascript
const fs = require('fs');
const path = require('path')
let filePath = path.resolve(__dirname, 'test.md')
fs.unlink(filePath, (err) => {
    if (err) throw err;
    console.log('已成功地删除文件');
});
```

### promise

#### 手动promise

```JavaScript
const fs = require('fs');
const path = require('path')
let filePath = path.resolve(__dirname, 'test.md')

function myReadFile(filePath) {
    new Promise((resolve, reject) => {
        fs.readFile(filePath, (err, data) => {
            data && resolve(data.toString())
            err && reject(err)
        })
    }).then(
        console.log,
        console.log
    )
}

myReadFile(filePath)
```

#### 官方promise

>  nodejs10以上版本支持

```javascript
// const fs = require('fs/promises');
const fs = require('fs').promises;
let filePath = path.resolve(__dirname, 'test.md')
(async function (path) {
    try {
        await fs.unlink(path);
        console.log(`已成功地删除文件 ${path}`);
    } catch (error) {
        console.error('出错：', error.message);
    }
})(filePath);
```

### 开始增删查改

```

```



## stream

## 多进程(child_process)

## 多线程(worker_threads)

[文章](https://zhuanlan.zhihu.com/p/74879045) 里面还讲了 cluster（集群）

## 第三方库

![20201203221904211](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201203221904211.png)

### chalk

>   语法： `chalk.颜色.其他样式`  或者`chalk.keyword('颜色')`

```javascript
const chalk = require('chalk');
let log = console.log;

// 字符串
log(chalk.red.underline('helloworld'));
log(chalk.blue.bold('helloworld'));
log(chalk.blue.bgRed('error') + ': sorry');
log(chalk.green('i love you'));
log(chalk.green('i love you,', chalk.white('so don\'t green me')));

// 模板字符串
let cat = {
    name: 'lulu',
    age: '8'
}

log(`
    名字${chalk.redBright(cat.name)}
    年龄${chalk.yellow(cat.age)}
`);

log(chalk`
    名字{redBright ${(cat.name)}}
    年龄{yellow ${(cat.age)}}
`);

// 占位符：%s
log(chalk.red('名字 %s'), cat.name);

// 主题
let error = chalk.white.bgRed;
let warn = chalk.keyword('orange');

log(error('i feel sorry'));
log(warn('warning'));
```

效果如下：

<img src="https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201203222227491.png" alt="20201203222227491" style="zoom:67%;" />

# 简单的脚手架搭建

## 1、初始化项目并安装依赖

```bash
npm init -y #初始化npm，全部填写yes
npm install commander git-clone shelljs open #安装需要的依赖
```
| 库          | 描述（实现多平台兼容的库） |
| ----------- | -------------------------- |
| `commander` | 命令行库，快速开发命令行   |
| `git-clone` | 下载库，克隆远程仓库       |
| `open`      | 打开浏览器的指定`url`      |
| `shelljs`   | 多平台的`shell`脚本        |

`shelljs`**官方是这样介绍的**

>		ShellJS is a portable (Windows/Linux/OS X) implementation of Unix shell commands on top of the Node.js API. You can use it to eliminate your shell script's dependency on Unix while still keeping its familiar and powerful commands. You can also install it globally so you can run it from outside Node projects - say goodbye to those gnarly Bash scripts!

机翻：
>		ShellJS是在Node.js API之上的Unix shell命令的可移植（Windows / Linux / OS X）实现。您可以使用它来消除shell脚本对Unix的依赖性，同时仍然保留其熟悉且功能强大的命令。您还可以全局安装它，以便可以从Node项目外部运行它-告别那些讨厌的Bash脚本！

## 2、测试配置

`#!/usr/bin/env node  `让该文件被shell调用，环境是node

```javascript
#!/usr/bin/env node    
console.log('hello world!');
```

在`package.json`

```json
  "bin": {
    "mycli": "./10other-pkg/commander/app.js"
  },
```

```bash
npm link #将文件进入全局执行（本次中的是app.js）
mycli #就能看到输出的helloworld
```

## 3、完成克隆、运行、全览三个功能

**这里以预览版webpack项目为例**

```
commander.version()
commander.commmand(string<此处填入参数>).description(string).action(cb)
shell.cd()
shell.exec()
```



```javascript
#!/usr/bin/env node
const program = require('commander');
const shell = require('shelljs');
const download = require('git-clone');
const open = require('open');
const { spawn } = require('child_process')

// 定义程序版本
program.version('1.0.0');
// 定义命令
// create pro
program.command('new <name>')
    .description('create project')
    .action(name => {
        let giturl = 'https://github.com/vuejs/vue-next-webpack-preview.git';
        download(giturl, `./${name}`, () => {
            shell.rm('-rf', `${name}/.git`);//删除.git日志

            shell.cd(name);//进入项目
            shell.exec('npm install');//安装依赖


            // 控制台输出操作提示
            console.log(`
            create ${name} success ！

            cd ${name}      :enter dir
            mycli run       :run the project
            mycli start     :overview the project
            `);
        })

    })
// run proj
program.command('run')
    .description('run project')
    .action(() => {
        // shell.exec('npm run dev');
        // 组合命令,优点：能丰富控制台的功能，输出友好的提示信息
    
        // let cp = spawn('npm', ['run', 'dev']);
        const cp = spawn(/^win/.test(process.platform) ? 'npm.cmd' : 'npm', ['run',  'dev']);
    
        // 打印运行信息
        cp.stdout.pipe(process.stdout);
        cp.stderr.pipe(process.stderr);
        cp.on('close', () => {
            console.log('start proj success !');
        })
    })

// overview,打开页面预览项目
program.command('overview')
    .description('overview project')
    .action(() => {
        open(`http://localhost:8080/`)
        console.log(`overview this project `);
    })


// 解析命令行传入的参数
program.parse(process.argv)
```

<img src="https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201203225412886.png" alt="20201203225412886" style="zoom:67%;" />

### 执行

```bash
mycli new app
mycli run
mycli overview
```

其实`npm run dev`之后就有`url`出现了，而且`mycli overview`这条命令我觉得太长了。

## 4、封装思路

**①一般就是将功能一般化、通用化后续使用只需要修改配置信息就能完成对应功能，不过这也只是我的简单猜想，等我工作了再补上实际操作。**

**②拆除`node_modules`的冗余文件。**

## 5、常见问题

关于`spawn`能力在Windows下失效的解决方式

```bash
events.js:161
  throw er; // Unhandled 'error' event
  ^
  
Error: spawn npm ENOENT
```

[stackoverflow里给出的方案](https://stackoverflow.com/questions/43230346/error-spawn-npm-enoent)

# 简单爬虫

| api                                                   | 描述                      |
| ----------------------------------------------------- | ------------------------- |
| `request(url,(error, response, body)=>{doSomething})` | 发起`http`请求            |
| `cheerio.load(body, { decodeEntities: false })`       | 解析响应体，`body`返回`$` |

```bash
npm install express -S
npm install request cheerio -D
```

`robots.txt` 是爬虫协议，指网页允许被爬取的范围 直接在域名后面添加 `/robots.txt`即可查看，通常不能查看的部分都会显示为 **403** 或 **404**

如： [sohu](https://www.sohu.com/robots.txt)里面有一条有趣的信息：`User-agent: Baiduspider`，禁止百度爬虫爬取

```javascript
const express = require('express')
const app = express()
const request = require('request')
const cheerio = require('cheerio')
app.get('/', (req, res) => {
    request('https://sports.sina.cn/nba/2020-11-10/detail-iiznctke0618872.d.html?cre=tianyi&mod=wspt&loc=-2&r=-1&rfunc=18&tj=cxvertical_wap_wspt&tr=73&vt=4&pos=10', function (error, response, body) {
        if (!error && response.statusCode == 200) {
            const $ = cheerio.load(body, { decodeEntities: false })//
            let title = $('h1').text()
            let avatar = `http:${$('.weibo_img img')['0'].attribs.src}`
            let author = $('.weibo_user').text()
            let time = $('.weibo_time').text().trim()
            let abstract = $('.art_p').text()
            res.json({
                "user": {
                    "author": author || '未命名的文章',
                    "avatar": avatar || "https://avatars1.githubusercontent.com/u/63615089?s=400&v=4"
                },
                "title": title || "未命名的标题",
                "createAt": `${new Date().getFullYear()}年${time}`,
                "content": abstract || '',
                "comments": [
                    {
                        "user": 'lala',
                        "commont": "文章还行"
                    }
                ]
            })
        }
    });
})
app.listen(3000, () => {
    console.log(`http://localhost:3000`);
})
```

