# \[js\]异步编程

# 重要知识速记

异步编程的方案

## 1、回调函数，回调地狱

```javascript
function a(){
	let a = 'a',
		b = 'b' 
	console.log(a)
	return b
}

function b(str){
	console.log(str)
}

b(a()) // a b
```

## 2、`yield`(生产)- 生成器函数

​    	声明`function* a(){}` yield可以暂停和恢复生成器函数
​		执行生成器函数他会返回一个迭代器，`iterator`上面挂载着`next(),`可以让`yield`暂停的内容恢复生产

## 3、`Promise`

**解决回调地狱，但依旧是异步代码的格式**

A+规范主干思路
		1、Promise(回调函数) 回调函数是执行器（excutor），会执行resolve和reject两个函数

​			1.1、出现错误就是reject【excutor错误、then的错误】

​		2、then中传入的不是回调函数，需要二次封装

​		3、then传入的状态是PENDING时需要依赖收集等【resolve/reject】执行

​		4、then执行后必须返回新的Promise

​		5、返回值一定是Promise，

​				5.1普通值（含undefined）或者promise的`FULFILLED`状态

​				5.2出现错误，或者promise的`REJECTED`状态

​		`Promise.all()` 
​			args：promise数组（**如果不是promise对象会被自动封装成promise对象**），不按执行顺序，按参数传入顺序返回【需要全部都是resolve才能】
​    	`Promise.allSettled()` 
​    		与上面相同，但是不会【错误中断】
​    	~~Promise.any()~~ 
​        	只要有一个是正确的就返回
​    	`Promise.then()`
​    	`Promise.catch(callback)` === `Promise.then(null, ()=>{callback()})`
​    	`Promise.finally()`
​    	`Promise.race【竞速】()`
​        	返回最先执行的那个
​    	`Promise.resolve()`
​    	`Promise.reject()`

## async await

```javascript
4、async await【长得像同步代码】
		async 返回promise，即可以调用上面的全部方法
		await 让代码暂停，直到右侧的代码执行完成才继续，【右侧代码一般是promise】
官方示例
fetch('coffee.jpg')
.then(response => response.blob())
.then(myBlob => {
  let objectURL = URL.createObjectURL(myBlob);
  let image = document.createElement('img');
  image.src = objectURL;
  document.body.appendChild(image);
})
.catch(e => {
  console.log('There has been a problem with your fetch operation: ' + e.message);
});


async function myFetch() {
  let response = await fetch('coffee.jpg');
  let myBlob = await response.blob();

  let objectURL = URL.createObjectURL(myBlob);
  let image = document.createElement('img');
  image.src = objectURL;
  document.body.appendChild(image);
}

myFetch()
.catch(e => {
  console.log('There has been a problem with your fetch operation: ' + e.message);
});	
```



>  因为JavaScript是一门单线程的编程语言，为了提高效率得使用异步编程，异步编程指使用`setTimeOut`等函数滞后某些需要花费大量时间，同时避免了部分阻塞或者异常导致后面的代码完全失效。
>
>  但是异步编程的代码出现了一个问题：回调地狱，使得后续的代码维护困难，下面介绍几个解决回调地狱的方案【注：这几种方案无疑会使得代码量增大，所以可以自行权衡】。
>
>  使用场景：网络请求、数据库连接操作、文件操作……
>
>  下面是几个简单的示例，和基本api示例，更多的则请访问MDN查看。

# yield（es6）

> 生成器函数：使用迭代器实现异步编程

## 语法

```JavaScript
       function * gen(){
            console.log(111);
            yield '一只没有耳朵';
             console.log(222);
            yield '一只没有尾部';
             console.log(333);
            yield '真奇怪';
             console.log(444);
        }

        let iterator = gen();
        console.log(iterator.next()); 
		console.log('-------------------------------------------');
        console.log(iterator.next()); 
		console.log('-------------------------------------------');
        console.log(iterator.next()); 
		console.log('-------------------------------------------');
        console.log(iterator.next());  
        console.log('-------------------------------------------');
        //遍历
        for(let v of gen()){
            console.log(v);// 输出yield的内容
            console.log('-------------------------------------------'); 
        }
```



## 使用场景

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

# Promise\(es6\)

> 依旧是异步代码的形式，但是能进行代码拆分，可读性比回调强上上。[more](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)

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

## PromiseA+规范

```javascript
const PENDING = 'PENDING',
    FULFILLED = 'FULFILLED',
    REJECTED = 'REJECTED';
/**
 *
 * @param {new Promise} promise2
 * @param {any} x return value
 * @param {function} resolve 新的promise的解决函数
 * @param {function} reject 新的promise的拒绝函数
 */
function resolvePromise(promise2, x, resolve, reject) {
    if (x === promise2) {
        return reject(new TypeError('Chaining cycle detected for promise # < MyPromise >'))
    }
    let called = false;
    if (typeof (x) === 'object' && x !== null || typeof (x) === 'function') {// 函数或者对象
        try {
            let then = x.then;
            // 确定是promise 因为x包含then方法
            if (typeof then === 'function') {
                then.call(x, (y) => {
                    if (called) return;
                    called = true;
                    // 递归解决嵌套问题
                    resolvePromise(promise2, y, resolve, reject)
                }, (r) => {
                    if (called) return;
                    called = true;
                    reject(x);
                })
            } else {// 不是promise
                resolve(x)
            }
        } catch (error) {
            if (called) return;
            called = true;
            reject(error)
        }
    } else {
        resolve(x)// 普通值
    }
}
class MyPromise {
    constructor(executor) {
        this.status = PENDING;
        this.value = undefined;
        this.onFulfilledCallbacks = [];
        this.onRejectedCallbacks = [];
        // 每个对象都有的内容
        const resolve = (value) => {
            if (this.status == PENDING) {
                this.status = FULFILLED;
                this.value = value;
            }
            // 发布
            this.onFulfilledCallbacks.forEach(fn => { fn(); })
        }
        const reject = (reason) => {
            if (this.status == PENDING) {
                this.status = REJECTED;
                this.value = reason;
            }
            // 发布
            this.onRejectedCallbacks.forEach(fn => { fn(); })
        }

        try {
            executor(resolve, reject)
        } catch (e) {
            reject(e);
        }
    }
    // 原型链上的函数
    // x 可能普通值也可能是promise
    then(onFulfilled, onRejected) {
        // 解决空参问题,即then穿透
        onFulfilled = typeof onFulfilled === 'function' ? onFulfilled : value => value;
        onRejected = typeof onRejected === 'function' ? onRejected : reason => { throw reason };
        let promise2 = new MyPromise((resolve, reject) => {
            if (this.status == FULFILLED) {
                setTimeout(() => {
                    try {
                        let x = onFulfilled(this.value);
                        resolvePromise(promise2, x, resolve, reject);
                    } catch (error) {
                        reject(error);
                    }
                }, 0)

            } if (this.status == REJECTED) {
                setTimeout(() => {
                    try {
                        let x = onRejected(this.value);
                        // 使用异步是等待promise2实例化
                        resolvePromise(promise2, x, resolve, reject);
                    } catch (error) {
                        reject(error);
                    }
                }, 0)

            }
            else if (this.status == PENDING) {
                // 订阅
                // 这里不用延时是等待改变状态才执行
                this.onFulfilledCallbacks.push(() => {
                    try {
                        let x = onFulfilled(this.value);
                        resolvePromise(promise2, x, resolve, reject);
                    } catch (error) {
                        reject(error);
                    }
                })
                this.onRejectedCallbacks.push(() => {
                    try {
                        let x = onRejected(this.value);
                        resolvePromise(promise2, x, resolve, reject);
                    } catch (error) {
                        reject(error);
                    }
                })
            }
        })
        return promise2;
    }

    catch(errorCallback) {
        return this.then(null, errorCallback);
    }
}


module.exports = MyPromise;
```



## allSettled

```JavaScript
Promise.myAllSettled = function (promises) {
    // 适合互不关联的情况，就算reject也不会终止执行

    return new Promise((resolve, reject) => {
        let elCount = 0;
        const result = [];
        function addElementToResult(index, elem) {
            result[index] = elem;
            elCount++;
            // 由于执行顺序是不定的所以result包含空元素，当相等时空元素也被填满
            if (elCount === result.length) {
                resolve(result);
            }
        }

        let index = 0;
        // 跟all一样处理promises，但是不会执行reject(),只执行封装之后的resolve
        for (const promise of promises) {
            const curIndex = index;
            promise.then(
                (value) => addElementToResult(
                    curIndex, {
                    status: 'fulfilled',
                    value
                }),
                (reason) => addElementToResult(
                    curIndex, {
                    status: 'rejected',
                    reason
                }));
            index++;
        }


        if (index === 0) {
            resolve([]);
            return;
        }
    });
}

var p1 = new Promise(resolve => {
    setTimeout(() => {
        resolve(1);
    }, 1000);
});
var p2 = new Promise(resolve => {
    setTimeout(() => {
        resolve(2);
    }, 2000);
});

var p4 = new Promise((resolve, reject) => {
    setTimeout(() => {
        reject('我是错误4');
    }, 500);
});

Promise.allSettled([p1, p2, p4]).then(values => console.log(values));
Promise.myAllSettled([p1, p2, p4]).then(values => console.log(values));
```



## all

```JavaScript
Promise.myAll = function (promises) {
    // 适合彼此依赖的情况
    // 还可以进行优化，传入数组中的元素中有不是Promise对象的可以封装成对象
    for (let i = 0; i < promises.length; i++) {
        if (!(promises[i] instanceof Promise)) {
            new Promise((res, rej) => {
                res(promises[i]);
            })
        }
    }
    return new Promise((resolve, reject) => {
        let haveError = false;
        let index = 0;
        let runCount = 0;
        const values = [];
        for (const promise of promises) {
            // 这里需要存储执行的promises的index，
            // 异步执行会导致执行时的index == promises.length
            let curIndex = index;
            promise.then(value => {
                if (haveError) return;
                values[curIndex] = value;
                runCount++;
                if (runCount === promises.length) {
                    resolve(values);
                }
            }, err => {
                if (haveError) return;
                haveError = true;
                reject(err)
            })
            index++;
        }
        if (index === 0) {
            resolve([]);
            return;
        }
    });
}




var p1 = new Promise(resolve => {
    setTimeout(() => {
        resolve(1);
    }, 1000);
});
var p2 = new Promise(resolve => {
    setTimeout(() => {
        resolve(2);
    }, 2000);
});
var p3 = new Promise(resolve => {
    setTimeout(() => {
        resolve(3);
    }, 500);
});

var p4 = new Promise((resolve, reject) => {
    setTimeout(() => {
        reject('我是错误4');
    }, 500);
});

// 与执行顺序无关，用户怎么写入就按什么顺序打印
Promise.all([p1, p2, p3]).then(values => console.log(values));
Promise.myAll([p1, p2, p3]).then(values => console.log(values));
```



## race

```JavaScript
Promise.myRace = function (promises) {
    return new Promise((resolve, reject) => {
        // 添加状态判断，只要执行过了就关闭状态
        let status = false;
        for (const promise of promises) {
            if (status) return;
            promise.then(
                (value) => {
                    if (status) return;
                    status = true;
                    resolve(value);
                },
                (err) => {
                    if (status) return;
                    status = true;
                    reject(err);
                });
        }
    });
}

var p1 = new Promise(resolve => {
    setTimeout(() => {
        resolve(1);
    }, 1000);
});
var p2 = new Promise(resolve => {
    setTimeout(() => {
        resolve(2);
    }, 2000);
});
var p3 = new Promise(resolve => {
    setTimeout(() => {
        resolve(3);
    }, 500);
});

// 谁快执行谁
Promise.race([p1, p2, p3]).then(values => console.log(values));
Promise.myRace([p1, p2, p3]).then(values => console.log(values));
```



# async-await\(es8\)

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

