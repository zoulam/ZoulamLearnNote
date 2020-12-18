# \[js\]Object和Reflect

### this指向问题

> this指针是指存在于非箭头函数的函数中的内容

#### 1、函数

未被实例化window/global

实例化：实例化的对象

'use strict':undefined，等待指定（实例化）

#### 1.1、对象内的函数

指向指向这个对象

```javascript
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

```javascript
// 00test1.js
var o = {
    a: 10,
    b: {
        a: 12,
        fn: function () {
            console.log(this.a);
            console.log(this);
        },
    },
}
var j = o.b.fn;
j();// j 在全局作用域  undefined , window/global
o.b.fn(); // 这是普通函数 12 ,o.b
```

```javascript
var bar = {
    myname: 'bar',
    getName: function () {
        console.log(myname);
        console.log(this);
    }
}

function foo() {
    var myname = 'foo';
    return bar.getName;
}
var myname = 'global';
var __printName = foo();

__printName(); // 'global' window/global

bar.getName(); // 'global' bar // myname 是 global 是因为没有this的时候默认向全局找
```

#### 2、节点元素

哪个节点绑定事件，this就指向该节点

#### 3、`call/apply`  and `bind`

|  | call | apply | bind |
| :--- | :--- | :--- | :--- |
| 参数 | `(obj, ...args)` | `(obj, [])` | `(obj, ...agrs)` |
| 执行 | 立即执行 | 立即执行 | 等待执行，返回一个新函数，使用在`addEventListener` |

#### 5、箭头函数

外层作用域（即可能是：1、全局，2、外层函数，3、外层对象）

```javascript
let name = "global"
let obj = {
    name: 'A',
    age: 18,
    say: () => {
        console.log(name);// global
        console.log(age);//ReferenceError: age is not defined
    }
}

obj.say();
```

> “箭头函数”的`this`，总是指向定义时所在的对象，而不是运行时所在的对象。
>
> 箭头函数的 `this` 的值是他的 lexical scope 的 `this` 的值。然而很多人并不明白什么是 lexical scope
>
> lexical scope：词法作用域

[关于箭头函数this的争议](https://github.com/ruanyf/es6tutorial/issues/150)

[MDN的标准答案](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

**箭头函数表达式**的语法比**函数表达式**更简洁，并且没有自己的`this`，`arguments`，`super`或`new.target`。箭头函数表达式更适用于那些本来需要匿名函数的地方，并且它不能用作构造函数。

**我的理解**：箭头函数没有this指针，this只是一个普通变量，它会一直顺着作用域去寻找this。

通俗的说法，**this的指向跟外层函数相同**

专业的说法，箭头函数是顺着自己的词法作用域去寻找this指针

## 操作Object的常用方法

### `assign(obj1,obj2)`

`arg2` 覆盖 `arg1` 的同名内容 **\[用于合并配置项\]**

### `obj.hasOwnProperty(key)`

判断该属性是否存在在对象内

### `GetPrototypeOf(obj)`

等效 `obj.__proto__`

```javascript
function Animal() {}
Animal.prototype.say = function () {
    console.log('hello');
}
let dog = new Animal();

//1.获取原型 [[GetPrototypeOf]]
//函数式的操作方法，函数有维护方便的优点
let proto = Object.getPrototypeOf(dog);

console.log(proto);
console.log(dog.__proto__);
```

### `stePrototpeOf(obj,{})`

实现附加原型链，添加在对象下的第一层 `__proto__` 下

### `Object.isExtensible(obj)`

返回Boolean判断对象是否具有扩展性

### `GetOwnProperty`

是一类function，取得原生的属性，而不是从原型链上继承的

#### `Object.getOwnPropertyDescriptor(obj,prop)`

如：可枚举、可写

> 查看`obj`的`prop`的描述

#### `Object.getOwnPropertyDescriptors(obj)`

> #### 获取对象的参数设置，实现深拷贝

#### `Object.getOwnPropertyNames(obj)`

> 以数组的形式返回属性名

#### `Object.getOwnPropertySymbols(obj)`

> 以数组形式，返回对象的`Symbol`的属性，

### `PreventExtensions`

禁止对象扩展，禁止添加，但是可以删除

`Object.preventExtensions(obj)`

### `DefineOwnProperty`

> 实现**Vue2.0**双向绑定的关键API

```javascript
defineOwnProperty(obj, key, {
    get(){},
        set:()=>{// set 方法一般需要写成箭头函数，因为需要获取指定的this

        }
})
```

### 获取，设置，删除

```javascript
// [[get]]
var obj = {
    a: 1,
    b: 2
}

console.log('b' in obj);
console.log('c' in obj);
console.log(obj.a);

// [[set]]
obj.a = 3;
obj['b'] = 5;
console.log(obj);//{ a: 3, b: 5 }

// [[delete]]

delete obj.a;
console.log(obj);//{ b: 2 }
```

### Enumerate

枚举

`obj.propertyIsEnumerable(prop)` 返回boolean

返回 `prop`是否实现了`iterator`

```javascript
var obj = {
    a: 1,
    b: 2
}
for (var k in obj) {
    console.log(k + ':' + obj[k]);
    // a:1
    // b:2
}
```

### 获取keys values entris

> `keys`和`values`返回的是数组

```javascript
var obj = {
    a: 1,
    b: 2
}

Object.setPrototypeOf(obj, { c: 3, d: 4 })
console.log(Object.keys(obj));//[ 'a', 'b' ]
console.log(Object.values(obj));//[ 1, 2 ]
```

官方的示例

```javascript
const object1 = {
  a: 'somestring',
  b: 42
};

for (const [key, value] of Object.entries(object1)) {
  console.log(`${key}: ${value}`);
}

// expected output:
// "a: somestring"
// "b: 42"
// order is not guaranteed
```

### `create(obj)`

浅拷贝对象，即原对象提供原型链

### `Object.fromEntries(entries)`

> 将键值对转化为对象

```javascript
const entries = new Map([
  ['foo', 'bar'],
  ['baz', 42]
]);

const obj = Object.fromEntries(entries);

console.log(obj);
// expected output: Object { foo: "bar", baz: 42 }
```

## Reflect

[推荐文章](https://juejin.im/post/6844903511826645006)

