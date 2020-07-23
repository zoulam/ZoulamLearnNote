# 数据类型

[难以置信的JavaScript真值表](https://thomas-yang.me/projects/oh-my-dear-js/)

## ①变量

### 声明方式

var\(variable\)

`var value = x;`

let\(让、使、令\)

`let value = 10;`

**语义：将10赋值给value**

const\(常量\)

`const PIXEL = 1024 * 1980;`

### const常见问题

const 声明的**引用数据类型**是否可以修改？

答：可以，const声明**引用数据类型**时是指向地址的而非具体值，所以是可以修改的。

### 三者区别

var的是ES6之前的JavaScript声明变量的方式，存在许多缺陷，比如：变量提升，作用域情况多变，如：误声明全局变量，for循环输出直接输入结果，比起let声明方式只有单手输入这个优点了。

误声明全局变量`var a = b = 1;`其中`b`被挂载到全局

### 数据存储的简单理解

```javascript
var num1 = 3;
var num2 = num1;//直接复制值
var num1 = 5;//原有再栈空间的num1未被清除
//变量指向新申请的栈空间
console.log(num1);//5
console.log(num2);//3

var a = [1, 2, 3, 4, 5];
var b = a;//从栈中复制了一份a指向堆空间的指针
a.push(6);//在堆内存操作
console.log(a);//[ 1, 2, 3, 4, 5, 6 ]
console.log(b);//[ 1, 2, 3, 4, 5, 6 ]
var a = [];//先在堆内存创建空数组 [] ,
//再将数组a放在栈内的删除并重新定义（注：此时的空间并为释放）
//然后将重新定义的a指向新的空数组，所以b不受影响
console.log(a);//[]
console.log(b);//[ 1, 2, 3, 4, 5, 6 ]
```

## ②类型判断

### 数据类型

> 基本数据类型：`number`、`undefined`、`null`、`string`、`boolean`、`Symbol`、`bigInt`
>
> `number`：`NaN`、`Infinity`
>
> ```javascript
> console.log(1 / 0);//Infinity
> console.log(-1 / 0);//-Infinity
> ```
>
> 基本数据类型的真值表
>
> | 数据类型 | 真值 | 假值 |
> | :--- | :--- | :--- |
> | boolean | true | fasle |
> | string | 任何非空字符串 | "" |
> | number | 非0的数值（包括无穷大） | NaN和0 |
> | object | 任何对象 | null |
> | undefined | 不适用 | undefined |
>
> 引用数据类型：`object`包含以下内容：
>
> **Function**：`function(){}`、 `()=>{}`、class内：`functionName(){}`
>
> **直接定义的对象**：`{}`
>
> **Array**：`[]`
>
> **Map**、
>
> **各类构造函数**

### typeof

虽然是命令操作符，但建议使用function的方式调用，返回值是`String`,即两层`typeof`后输出的内容一定是string

```javascript
//node.js环境
// console.log(a);//ReferenceError: a is not defined
console.log(typeof (a));//undefined
console.log(typeof([]));//object
console.log(typeof({}));//object
console.log(typeof(null));//object
console.log(typeof(RegExp()));//object
console.log(typeof(undefined));//undefined
console.log(typeof(false));//boolean
console.log(typeof(""));//string
console.log(typeof(NaN));//number
console.log(typeof(95));//number
console.log(function(){});//[Function] 浏览器中：f(){}
console.log(typeof(Math));//object

//typeof返回值
console.log(typeof (typeof (a)));//string
console.log(typeof (typeof (123)));//string
```

### instanceof

```javascript
//缺陷：无法判断基本数据类型，且所有的值都可以理解为实例化于Object,而且属于猜测性判断
console.log([] instanceof Array);//true
console.log({} instanceof Object);//true
console.log(function(){} instanceof Object);//true
console.log(new Date() instanceof Date);//true
// console.log(undefined instanceof Undefined);//true
// ReferenceError：Undefined is not defined
```

### constructor\(包装类`String()`、`Number()`、`Boolean()`\)

```javascript
// 需要配合包装类判断，三个基本数据类型，String、Number、Boolean

console.log("".constructor == String);//true

// 不准确，哪怕是塞个String进去也是一样的结果
console.log(Number(5).constructor == Number);//true
// console.log(5.constructor == Number);//语法错误
// 不准确
console.log(Boolean(5).constructor == Boolean);//true
```

### Object.prototype.toString.call\(\)\[稳定准确\]

```javascript
// node.js
Object.prototype.toString.call('') ;   // [object String]
Object.prototype.toString.call(1) ;    // [object Number]
Object.prototype.toString.call(true) ; // [object Boolean]
Object.prototype.toString.call(Symbol()); //[object Symbol]
Object.prototype.toString.call(undefined) ; // [object Undefined]
Object.prototype.toString.call(null) ; // [object Null]
Object.prototype.toString.call(new Function()) ; // [object Function]
Object.prototype.toString.call(new Date()) ; // [object Date]
Object.prototype.toString.call([]) ; // [object Array]
Object.prototype.toString.call(new RegExp()) ; // [object RegExp]
Object.prototype.toString.call(new Error()) ; // [object Error]
// Object.prototype.toString.call(document) ; // [object HTMLDocument]
Object.prototype.toString.call(global) ; //[object global] window 是全局对象 global 的引用
console.log('-------------------------------------------');
```

### 自行封装

```javascript
function getType(obj){
    let type  = typeof obj;
    if(type != "object"){
      return type;
    }
    return Object.prototype.toString.call(obj).replace(/^\[object (\S+)\]$/, "");
    // 使用正则将 [object ]剔除
  }

  console.log( getType(''));//String
```

## ③类型转换

### 1\)隐式类型转换\(自动\)

> 聪明的JavaScript引擎通过猜测代码行为进行类型转换。

| 情况 | 效果 |
| :--- | :--- |
| 加号：`+` | `'b' + 1`字符串拼接**str   num=&gt;str** |
| 减乘除取模`- * / %` | `2 * "3"`   **str num =&gt;num**                    `2 * "wu"`  **str num =&gt; num\(NaN\)** |
| 正负号`- +` | `+"wuhu"`  **str =&gt;num\(NaN\)** |
| 自加、自减`++ --` | `"123"++` **str=&gt;num\(124\)** |
| 比较运算符号`> < >= <=` | ①字符与字符转化为num类型的对应ASCII值再**逐个比较，前面的权重大** ②数字与字符**str=&gt;num** |

逐一比较逻辑

`"1.5" > "11"`\(false\)，`"4.5" > "11"`\(true\)

比较逻辑 1---1（相等） .---1 （1大）"1.5" &lt; "11"

比较逻辑 4---1（1大） 即"4.5" &gt; "11"

```javascript
//隐式类型转换
//String拼接 num——>str
console.log("字符串拼接："+'b' + 1);//b1
console.log('-------------------------------------------');

// - * / % 都是Str -> num
process.stdout.write("- * / % :");
console.log(2 * "3");//6
console.log(2 * "wu");//NaN
console.log('-------------------------------------------');

//加等于减等于：Str -> num
var b ='123'
var c ="wuhu"
process.stdout.write("加减号实现隐式转换：");
console.log(`${typeof(+b)}:${+b}`);//number:123
console.log(`${typeof(-b)}:${-b}`);//number:-123
console.log(`${typeof(+c)}:${+c}`);//number:NaN
console.log(`${typeof(-c)}:${-c}`);//number:NaN
console.log('-------------------------------------------');

//自加
var a = '123';
var b = "wuhu";
// string++  Str -> num
process.stdout.write("自加：");
console.log(a++);//124
console.log(b++);//NaN
console.log('-------------------------------------------');

//比较运算符
process.stdout.write("比较运算符：");
console.log(1 < "wu");//false
console.log(1 < "2");//true str->num
console.log("a" > "b");//false str->num(ASCII)
console.log('-------------------------------------------');
```

#### undefined、0、null、NaN

```javascript
//undefined、null、0
console.log(undefined==0);//false
console.log(undefined>0);//false
console.log(undefined<0);//false
console.log(null==0);//false
console.log(null>0);//false
console.log(null<0);//false
console.log(undefined==null);//true
console.log(undefined===null);//false
console.log('-------------------------------------------');

//NaN的判断
console.log(NaN == NaN);//false
//所以定义了一个方法isNaN
//逻辑：①xx->num ②再判断
process.stdout.write("isNaN:");
console.log(isNaN(123));//false
console.log(isNaN("123"));//false
console.log(isNaN(null));//false
console.log(isNaN(NaN));//true
console.log(isNaN("bule"));//true(不能转化为数字)
console.log(isNaN("a"));//true
console.log(isNaN(undefined));//true
```

#### 比较运算符细节

```javascript
// 04类型转换细节.js

console.log("abcc" > "aa");//true
// a:97
// A:65
console.log("ab" > "aacddd");//true

console.log("a".charCodeAt());//97
// 逐个比较
console.log("23"<"3");//true
console.log("23"<3);//false

console.log('-------------------------------------------');
// 字符串与数字比较的逻辑
// 字符串12转化为number类型的12
console.log("12" == 12);// 转化后值相同：true
console.log("12" === 12);//false 数据类型不同

console.log('A' > 1);// false
console.log('A' <= 1);// false
console.log('a' > 1);// false
console.log('a' <= 1);// false

// 上面四行等价于
console.log(NaN > 1);// false
console.log(NaN <= 1);// false
console.log(NaN > 1);// false
console.log(NaN <= 1);// false
```

### 2\)强制类型转换

> 用户为了实现某些目的，而使用JavaScript内置的转换方式。

parse：**解析** 音标：/pa:z/

| 方式 | 效果（见下方代码） |
| :--- | :--- |
| `Number()` | 转化成数据类型 |
| `parseInt()` | 转化为整型，有去除尾部字符串的功能 |
| `parseFloat()` | 转化为浮点型 |
| `.toString()` | 转化为字符串 |

```javascript
//强转成number 不能转普通数值的变成NaN
var a = Number(3);//3
var a =  Number(true);//1
var a =Number(false);//0
var a = Number(null);//0
var a = Number("");//0
var a = Number(undefined);//NaN
var a = Number("123a");//NaN
var a = Number( "a");//NaN
var a = Number("a123");//NaN
var a = Number(3.14);//3.14

console.log('-------------------------------------------');

var a = parseInt(true);//number:NaN
var a = parseInt(false);//number:NaN
var a = parseInt(null);//number:NaN
var a = parseInt(undefined);//number:NaN
var a = parseInt("a");//number:NaN
var a = parseInt("a123");//number:NaN
var a = parseInt("123a");//number:123
var a = parseInt("1a23");//number:1
var a = parseInt(3.14);//number:3

//node.js环境
// 双参数 parseInt(解析内容, 解析方式) 解析方式：eg：16 以十六进制解析
console.log(parseInt("10", 16));//16
console.log(parseInt("10", 2));//2
console.log(parseInt("10", 8));//8
console.log(parseInt(11, "10"));//8
// 0x 十六进制
console.log(parseInt("0xA"));//10
console.log(parseInt("22.6"));//22
console.log(parseInt("070"));//70 浏览器环境下：56,以八进制解析
console.log(parseInt(070));//56 浏览器环境下：undefined

console.log('-------------------------------------------');

var b = '3.1415926';
// console.log(b.toFixed(2));
//b.toFixed is not a function,此时的b还是string类型,无法使用float类型的方法
var b = parseFloat(b);
console.log(b.toFixed(2));//四舍五入，保留两位小数
console.log(typeof (b));//number
console.log(typeof (b.toFixed(2)));//string
//return 新创建的string类型的3.14 但原来的未被改变

console.log('-------------------------------------------');

//转化为String
var c = 123;
console.log(typeof (String(c)));//string
console.log(typeof (c + ""));//string
console.log(typeof (c.toString()));//string

console.log('-------------------------------------------');

//一些坑
var d = 1234;
console.log(d.length);//undefined
console.log(String(d).length);//4
```

## 常见问题

### Symbol的实现

猜测是使用**雪花算法**

### 面试小坑

```javascript
global.a || (global.a = '1')
console.log(global.a);//1
```

**本题的考点在于括号的优先级是最高的，所以先赋值，然后或的左边是真就停止，或逻辑右半部分终止。**

错误逻辑：`global.a`是假，所以执行`global.a = '1'`

