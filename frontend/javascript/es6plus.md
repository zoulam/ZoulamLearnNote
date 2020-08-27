# ES6+

## ES6+

## ES7

### includes、\*\*

```javascript
        // includes   原来的判断方式 indexOf 有则返回下标，没有返回 -1
        const mingzhu = ['西游记','红楼梦','三国演义','水浒传'];

        //判断
        console.log(mingzhu.includes('西游记'));
        console.log(mingzhu.includes('金瓶梅'));

        // **
        console.log(2 ** 10);//
        console.log(Math.pow(2, 10));
```

## ES8

### async、await

> 1、async的返回值是Promise对象，即**可以使用Promise内的方法**
>
> 2、Promise的结果由async函数的执行的返回值决定
>
> ①
>
> ​ 返回值只要不是返回错误，最终都会返回Promise对象且是fulfilled状态
>
> ​ 返回值是error，最终都会返回Promise对象且是rejected状态
>
> ②
>
> ​ 返回值是Promise对象，则由该对象的状态的决定
>
> 1、await必须写在async函数内
>
> 2、右侧的内容是Promise对象
>
> 3、返回的是Promise成功或者失败的值，为了保险起见包裹在`try…catch`内处理

```javascript
        //创建 promise 对象
        const p = new Promise((resolve, reject) => {
            // resolve("用户数据");
            reject("失败啦!");
        })

        // await 要放在 async 函数中.
        async function main() {
            try {
                let result = await p;//此处接收resolve或者reject传入的值
                console.log(result);
            } catch (e) {
                console.log(e);
            }
        }
        //调用函数
        main();
```

### 读写文件实践

```javascript
//1. 引入 fs 模块
const fs = require("fs");

//读取『为学』
function readWeiXue() {
    return new Promise((resolve, reject) => {
        fs.readFile("./resources/为学.md", (err, data) => {
            //如果失败
            if (err) reject(err);
            //如果成功
            resolve(data);
        })
    })
}

function readChaYangShi() {
    return new Promise((resolve, reject) => {
        fs.readFile("./resources/插秧诗.md", (err, data) => {
            //如果失败
            if (err) reject(err);
            //如果成功
            resolve(data);
        })
    })
}

function readGuanShu() {
    return new Promise((resolve, reject) => {
        fs.readFile("./resources/观书有感.md", (err, data) => {
            //如果失败
            if (err) reject(err);
            //如果成功
            resolve(data);
        })
    })
}

//声明一个 async 函数

// async function(){
// let variable = await Promise[Object];
// }

async function main() {
    //获取为学内容
    let weixue = await readWeiXue();
    //获取插秧诗内容
    let chayang = await readChaYangShi();
    // 获取观书有感
    let guanshu = await readGuanShu();

    console.log(weixue.toString());
    console.log(chayang.toString());
    console.log(guanshu.toString());
}

main();
```

### 对象方法的扩展

```javascript
        //声明对象
        const user = {
            name: "zoulam",
            age: 18
        };

        // 获取对象所有的键
        console.log(Object.keys(user));
        // 获取对象所有的值
        console.log(Object.values(user));
        // entries
        console.log(Object.entries(user));
        // 创建 Map
        const m = new Map(Object.entries(user));
        console.log(m.get('cities'));

        // 对象属性的描述对象
        console.log(Object.getOwnPropertyDescriptors(user));

        // 第一个参数的原型对象，第二个参数就是原型描述,用上面的方法获取，实现对象的深拷贝
        const obj = Object.create(null, {
            name: {
                //设置值
                value: 'zoulam',
                //属性特性
                writable: true,
                configurable: true,
                enumerable: true
            }
        });
```

## ES9

### 对象的rest

```javascript
        //rest 参数
        function connect({host, port, ...user}){
            console.log(host);
            console.log(port);
            console.log(user);
        }

        connect({
            host: '127.0.0.1',
            port: 3306,
            username: 'root',
            password: 'root',
            type: 'master'
        });


        //对象合并
        const skillOne = {
            q: '天音波'
        }

        const skillTwo = {
            w: '金钟罩'
        }

        const skillThree = {
            e: '崔筋断骨'
        }
        const skillFour = {
            r: '猛龙摆尾'
        }

        const mangseng = {...skillOne, ...skillTwo, ...skillThree, ...skillFour};

        console.log(mangseng)
```

### 正则扩展

#### 对正则的分组进行命名

```javascript
        //声明一个字符串
        // let str = '<a href="http://www.luluxi.com">zoulam</a>';

        // //提取 url 与 『标签文本』
        // const reg = /<a href="(.*)">(.*)<\/a>/;

        // //执行
        // const result = reg.exec(str);

        // console.log(result);
        // // console.log(result[1]);
        // // console.log(result[2]);

        // ?<name>
        let str = '<a href="http://www.luluxi.com">zoulam</a>';
        //分组命名
        const reg = /<a href="(?<url>.*)">(?<text>.*)<\/a>/;

        const result = reg.exec(str);

        console.log(result.groups.url);

        console.log(result.groups.text);
```

#### 反向断言

> 根据你前面的字符判断是否符合条件 `?<=`

```javascript
        //声明字符串
        let str = 'JS5211314你知道么555啦啦啦';
        //正向断言
        const reg = /\d+(?=啦)/;
        const result = reg.exec(str);

        //反向断言
        const reg = /(?<=么)\d+/;
        const result = reg.exec(str);
        console.log(result);
```

#### doAll

> 高效提取html标签的内容

```javascript
//dot  .  元字符  除换行符以外的任意单个字符
        let str = `
        <ul>
            <li>
                <a>肖生克的救赎</a>
                <p>上映日期: 1994-09-10</p>
            </li>
            <li>
                <a>阿甘正传</a>
                <p>上映日期: 1994-07-06</p>
            </li>
        </ul>`;
        //声明正则
        // const reg = /<li>\s+<a>(.*?)<\/a>\s+<p>(.*?)<\/p>/;
        const reg = /<li>.*?<a>(.*?)<\/a>.*?<p>(.*?)<\/p>/gs;
        //执行匹配
        // const result = reg.exec(str);
        let result;
        let data = [];
        while(result = reg.exec(str)){
            data.push({title: result[1], time: result[2]});
        }
        //输出结果
        console.log(data);
```

## ES10

### `Object.fromEntries`

```javascript
        //二维数组
        const result2 = Object.fromEntries([
            ['name','zoualm'],
            ['age', 19]
        ]);
        console.log(result2);
        //Map
        const m = new Map();
        m.set('name','luluxi');
        const result = Object.fromEntries(m);
        console.log(result);

        //Object.entries ES8 即互逆操作
        const arr = Object.entries({
            name: "zoulam"
        })
        console.log(arr);
```

### `String.prototype.trim`

```javascript
        // trim
        let str = '   iloveyou   ';

        console.log(str);
        console.log(str.trimStart());
        console.log(str.trimEnd());
        console.log(str.trimRight());
```

`Array.prototype.flat`

```javascript
        // flat 平
        // 将多维数组转化为低维数组
        const arr = [1,2,3,4,[5,6]];
        const arr = [1,2,3,4,[5,6,[7,8,9]]];
        // 参数为深度 是一个数字
        console.log(arr.flat(2));
```

```javascript
          //flatMap 是数组map() 和flat的结合
        const arr = [1,2,3,4];
        const result = arr.flatMap(item => [item * 10]);
        console.log(result);
```

### `Symbol.prototype.description`

```javascript
        //创建 Symbol
        let s = Symbol('zoulam');

        console.log(s.description);
```

## ES11\(暂时依托浏览器环境\)

### 私有属性

```javascript
        class Person{
            //公有属性
            name;
            //私有属性
            #age;
            #weight;
            //构造方法
            constructor(name, age, weight){
                this.name = name;
                this.#age = age;
                this.#weight = weight;
            }

            intro(){
                console.log(this.name);
                console.log(this.#age);
                console.log(this.#weight);
            }
        }

        //实例化
        const girl = new Person('晓红', 18, '45kg');


        // console.log(girl.name);
        // 内容无法获取
        // console.log(girl.#age);
        // console.log(girl.#weight);

        // 只能通过特殊方式获取
        girl.intro();
```

### `Promise.allSettled`

> 返回的值总是成功的，但是可以返回每个任务的具体状态
>
> `Promise.all`需要全部内容都成功才能成功

```javascript
        //声明两个promise对象
        const p1 = new Promise((resolve, reject)=>{
            setTimeout(()=>{
                resolve('商品数据 - 1');
            },1000)
        });

        const p2 = new Promise((resolve, reject)=>{
            setTimeout(()=>{
                // resolve('商品数据 - 2');
                reject('出错啦!');
            },1000)
        });

        //调用 allsettled 方法
        // 返回值始终是成功的
        const result = Promise.allSettled([p1, p2]);
        console.log(result);
```

![result](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200827112157879.png)

### `String.prototype.matchAll`

> 批量提取字符串内容（返回值是数组）

```javascript
        let str = `<ul>
            <li>
                <a>肖生克的救赎</a>
                <p>上映日期: 1994-09-10</p>
            </li>
            <li>
                <a>阿甘正传</a>
                <p>上映日期: 1994-07-06</p>
            </li>
        </ul>`;

        //声明正则
        const reg = /<li>.*?<a>(.*?)<\/a>.*?<p>(.*?)<\/p>/sg

        //调用方法
        const result = str.matchAll(reg);

        // for(let v of result){
        //     console.log(v);
        // }

        const arr = [...result];

        console.log(arr);
```

### 可选链操作符

> 提取对象中深度内容（不需要一层一层判断，如果不判断只要缺少值就报错）

```javascript
        function main(config){
            // const dbHost = config && config.db && config.db.host;
            const dbHost = config?.db?.host;

            console.log(dbHost);
        }

        main({
            db: {
                host:'192.168.1.100',
                username: 'root'
            },
            cache: {
                host: '192.168.1.200',
                username:'admin'
            }
        })
```

### 动态`import`

> es6的是静态`import`，即全部一次性导入
>
> 动态`import`，实现按需加载（懒加载），即：使用时才加载，不使用就不加载

`import.html`

```javascript
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>动态 import </title>
</head>
<body>
    <button id="btn">点击</button>
    <script src="./js/app.js" type="module"></script>
</body>
</html>
```

`app.js`

```javascript
// import * as m1 from "./hello.js";
//获取元素
const btn = document.getElementById('btn');

btn.onclick = function(){
    import('./hello.js').then(module => {
        module.hello();
    });
}
```

`hello.js`

```javascript
export function hello() {
    alert('Hello');
}
```

### `BigInt`

```javascript
        //大整形
        let n1 = 521n;
        console.log(n, typeof(n1));

        //函数
        let n2 = 123;
        console.log(BigInt(n2));
        // console.log(BigInt(1.2)); //error

        //大数值运算
        let max = Number.MAX_SAFE_INTEGER;
        console.log(max);
        console.log(max + 1);
        console.log(max + 2);

        console.log(BigInt(max));
        // console.log(BigInt(max) + 1); // erro bigInt数据类型只能和bigInt数据类型运算
        console.log(BigInt(max) + BigInt(1));
        console.log(BigInt(max) + BigInt(2));
```

### globalThis

> 始终指向全局的this
>
> 浏览器：window
>
> node.js:global

```javascript
console.log(globalThis);
```

