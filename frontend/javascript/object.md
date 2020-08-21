# Object和Reflect

## this指向问题

> this指针是指存在于非箭头函数的函数中的内容

### 1、函数

未被实例化window/global

实例化：实例化的对象

'use strict':undefined，等待指定

### 1.1、对象内的函数

指向指向这个对象

```JavaScript
let foo = {
    a: 3,
    bar() {
        console.log(this);
        console.log(this.a);
        console.log(this.a === foo.a);
        setTimeout(() => {
            console.log('-------------------------------------------');
            console.log(this);
        }, 100)
    }
}

foo.bar();
```



### 2、节点元素

哪个节点绑定事件，this就指向该节点

### 3、`call/apply`  and `bind`



### 5、箭头函数

外层作用域（即可能是：1、全局，2、外层函数，3、外层对象）

> “箭头函数”的`this`，总是指向定义时所在的对象，而不是运行时所在的对象。
>
> 
>
> 箭头函数的 `this` 的值是他的 lexical scope 的 `this` 的值。然而很多人并不明白什么是 lexical scope
>
>  lexical scope：词法作用域

[关于箭头函数this的争议](https://github.com/ruanyf/es6tutorial/issues/150)

[MDN的标准答案](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

**箭头函数表达式**的语法比**函数表达式**更简洁，并且没有自己的`this`，`arguments`，`super`或`new.target`。箭头函数表达式更适用于那些本来需要匿名函数的地方，并且它不能用作构造函数。

**我的理解**：箭头函数没有this指针，this只是一个普通变量，它会一直顺着作用域去寻找this。

​	通俗的说法，**this的指向跟外层函数相同**

## Object和Reflect

## 操作Object的14种常用方法

