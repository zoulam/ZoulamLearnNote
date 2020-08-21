# ES6+

# ES7

## includes、**

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

# ES8

## async、await

> 1、async的返回值是Promise对象，即**可以使用Promise内的方法**
>
> 2、Promise的结果由async函数的执行的返回值决定
>
> ①
>
> ​	返回值只要不是返回错误，最终都会返回Promise对象且是fulfilled状态
>
> ​	返回值是error，最终都会返回Promise对象且是rejected状态
>
> ②
>
> ​	返回值是Promise对象，则由该对象的状态的决定

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

## 读写文件实践

```JavaScript
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

