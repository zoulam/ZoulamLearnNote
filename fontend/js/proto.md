# \[js\]原型

## 重要知识速记

prototype是函数特有结构，`__proto__`是对象属性是原型链的链条

```javascript
function myNew(Func, ...args) {
    let instance = {}
    Object.setPrototypeOf(instance, Func.prototype)
    let res = Func.apply(instance, args)
    if (typeof res === 'function' || typeof res === 'object' && res !== null) {
        return res
    }
    return instance
}

function myInstanceof(obj, Func) {
    let proto = obj.__proto__
    let prototype = Func.prototype
    while (proto == null) {
        if (proto == prototype) {
            return false
        }
        proto = proto.__proto__
    }
    return false
}

Object.myCreate = function (p) {
    function f() { };
    f.prototype = p;
    return new f();
}
```

> 原型链的终点是`Object.prototype`

```javascript
function Cat(color, name) {
    this.name = name;
    this.color = color;
}

Cat.weight = 0;// 只能用构造函数.属性取得
Cat.height = 0;
Cat.prototype.height = '15cm';
let cat = new Cat('red', 'zoulam');
console.log(cat);
Cat.prototype.cute = true;// 写在实例化之后才能取得
Cat.prototype = {// 必须写在实例化之前才能被取得
    cute: false
}
console.log(cat.cute, cat.weight, Cat.weight, cat.height);
```

![&#x7ED3;&#x679C;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200822153912546.png)

## 原型链调用方式

从离自己最近的开始找顺着`__proto__`往上找，找到就返回，直到`Object.prototype`仍旧没有就返回`undefined`

关于同名内容的覆盖关系（其实就是原型链的顺序关系）

`内部this.[value] > Function.prototype.[value] == Function.prototype = { } > Function.[value]`

## 为原型添加内容的方式

1、使用`构造函数.属性/方法`的方式使用`构造函数.属性/方法`的形式调用，效果与**ES6**中的**static**语法一致。

如：`Promise.reject()` `Promise.resolve()`

**挂载到**```constructor``上

2、`构造函数.prototype.属性/方法`添加的内容，**需要实例化之后**，使用`对象.属性/方法`的方式调用。

实例化的过程：从**constructor中的prototype取出挂载到**对象的`__proto__`上，未被实例化则无法取出。

3、`this`添加的的内容直接挂载到与`__proto__`平级的位置

```javascript
function HandPhone(color, brand) {
    this.color = color;
    this.brand = brand;
}
// HandPhone.prototype.screen = '18:9';
// HandPhone.prototype.system = 'Android';
// HandPhone.prototype.rom = '64G';
// HandPhone.prototype.ram = '6G';
// HandPhone.prototype.screen = '16:9';//覆盖前面的
// HandPhone.prototype.call = function () {
//     console.log(this.brand + ":我是世界上最好的手机");
// }
//美观的书写
HandPhone.prototype = {
    screen: '18:9',
    system: 'Android',
    rom: '64G',
    ram: '6G',
    screen: '16:9',//覆盖前面的
    call: function () {
        console.log(this.brand + ":我是世界上最好的手机");
    }
}
var hp1 = new HandPhone('red', 'mi');
var hp2 = new HandPhone('black', 'huawei');
//继承
console.log(hp1.rom);//64G
console.log(hp2.ram);//6G
//不继承
console.log(hp1.screen);//16:9
console.log(hp2.screen);//16:9
hp2.call();//huawei:我是世界上最好的手机
```

## 继承（es6之前）

```javascript
var inherit = (function () {
    var Buffer = function () { }
    return function (Target, Origin) {
        Buffer.prototype = Origin.prototype;
        Target.prototype = new Buffer();// 建立中间对象，实现父类和子类不干扰
        Target.prototype.constructor = Target;// 指定子类的构造器
        Target.prototype.super_class = Origin;// 指定父类
    }
})();

Teacher.prototype.name = 'mike';
function Teacher() { }
function Student() { }
function Buffer() { }
inherit(Student, Teacher);
Student.prototype.age = 18;

var s = new Student();
var t = new Teacher();

console.log(s);// Student{}
console.log(s.__proto__);// Teacher{包含子类}
console.log(Student.prototype);// Teacher{包含子类}
console.log(s.__proto__ === Student.prototype);// true
console.log(t);// Teacher{不包含子类}
```

![&#x7ED3;&#x679C;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200822152058003.png)

```text
|this设置的值
|__proto__
    |Function.prototype.[value]的内容实例化之后的位置
    |constructor    
        |Function.[value]添加的值
        |prototype 里面包含着Function.prototype.[value]添加的值（未被实例化）
        |__proto__
    |super_class:继承的父亲构造函数
    |proto
        |constructor:默认指向构造函数
        |proto
            |constructor：Object()
            |操作对象的方法
            |……
            |……
            |……
```

## new的过程中发生了什么

1、执行this指向当前对象

`let benz = new Car()`即：将`Car`内的`this`指向`benz`

2、从`Function`（构造函数）上的`prototype`取出值到对象的`__proto__`

`console.log(benz .__proto__ === Car.prototype);// true`

