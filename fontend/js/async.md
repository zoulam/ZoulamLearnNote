# \[js\]异步编程

> ​ 因为JavaScript是一门单线程的编程语言，为了提高效率得使用异步编程，异步编程指使用`setTimeOut`等函数滞后某些需要花费大量时间，同时避免了部分阻塞或者异常导致后面的代码完全失效。
>
> ​ 但是异步编程的代码出现了一个问题：回调地狱，使得后续的代码维护困难，下面介绍几个解决回调地狱的方案【注：这几种方案无疑会使得代码量增大，所以可以自行权衡】。
>
> ​ 使用场景：网络请求、数据库连接操作、文件操作……
>
> ​ 下面是几个简单的示例，和基本api示例，更多的则请访问MDN查看。

## yield（es6）

> 生成器函数：使用迭代器实现异步编程

```javascript
        //模拟获取  用户数据  订单数据  商品数据
        function getUsers(){
            setTimeout(()=>{
                let data = '用户数据';
                //调用 next 方法, 并且将数据传入
                iterator.next(data);
            }, 1000);
        }

        function getOrders(){
            setTimeout(()=>{
                let data = '订单数据';
                iterator.next(data);
            }, 1000)
        }

        function getGoods(){
            setTimeout(()=>{
                let data = '商品数据';
                iterator.next(data);
            }, 1000)
        }

        function * gen(){
            //获取信息的过程是有先后顺序的，前面的信息和后面信息是对应的
            let users = yield getUsers();
            let orders = yield getOrders();
            let goods = yield getGoods();
            console.log(`users:${users},orders:${orders},goods:${goods}`);
        }

        //调用生成器函数
        let iterator = gen();
        iterator.next();
        console.log('i am normal function!');
```

## Promise\(es6\)

> 依旧是异步代码的形式，但是能进行代码拆分，可读性比回调要墙上不上。[more](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)

```javascript
// 05promise.js
//放入耗时长的代码块，让其他代码可以正常异步执行

//获取执行结果
// var promise = new Promise((resolve, reject) => {
//     setTimeout(() => {
//         resolve("run ok!")
//     }, 2000);
// })
// promise.then(value => {
//     console.log(value)
// });
// console.log("before promise");
// //before promise
// //run ok!

// console.log('-------------------------------------------');

// //异常捕获
// var promise2 = new Promise((resolve, reject) => {
//     setTimeout(() => {
//         reject("run false!")
//     }, 2000);
// })
// promise2.catch(error => console.log(error));


console.log('-------------------------------------------');
//链式调用
// new Promise((resolve, reject) => {
//     setTimeout(() => {
//         resolve(1);
//     }, 1000);
// })
//     .then(value => {
//         console.log(value);//1
//         return value + 10;
//     })
//     .then(value => {
//         console.log(value);//11
//         return new Promise(resolve => resolve(value + 20));
//     })
//     .then(value=>{
//         console.log(value);//31
//     })

//将所有的错误积累到最后catch

new Promise((resolve, reject) => {
    setTimeout(() => {
        // reject("promise false");
        resolve(1);
    }, 1000);
})
    .then(value => {
        console.log(value);
        // throw "then1 error"
        return value + 10;
    })
    .then(value => {
        console.log(value);
        return new Promise(resolve => resolve(value + 20));
    })
    .then(value => {
        console.log(value);
    })
    .catch(error => {
        console.log(error);                //未被注释前：promise false
        // 1
        // then1 error
    })
```

## async-await\(es8\)

> 较为知名的案例：koa

```javascript
async function async1() {
    let result2 = await async2();
    let result3 = await async3();
    try{
        let result4 = await async4();
    }catch(e){
        console.log(e);
    }
    console.log(result2);
    console.log(result3);
}
async function async2() {
    return new Promise(resolve => {
        setTimeout(() => {
            resolve(2);
        }, 1000)
    })
}

async function async3() {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve(3);
        }, 500)
    })
}

async function async4() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            reject("run error!")
        }, 500)
    })
}

async1();
//跟执行顺序无关
// run error!
// 2
// 3
```

