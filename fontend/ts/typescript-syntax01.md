# \[typescript\]syntax01

# 类型内容速览

```javascript
// 基础类型，声明方式是变量名: 类型
boolean number string symbol bigInt undefined null
// 引用类型(除了基础类型外都是对象)
object

// 特殊类型
any // 表示任何类型，可以在配置中禁用，适用于旧（JS）项目迁移过来的，忽略语法检查
unknow // 未知类型，不会忽略语法检查
void  // 函数返回值的undefined，即没有返回值
never // 作为函数返回值，表示里面有死循环永远无法到达的返回值

// 数组
// 类型[]
number[]
// Array<类型>
Array<string>
// 内置类型
ReadonlyArray<number> // 内部元素都不可以修改，可以理解为Object.freeze()
    
// 元组，解决数组只能放同一类型的问题
[类型1, 类型2, ……]    
    
// 类型断言
// 变量 as 类型
// <类型> 变量

// keyof，类型查询，即将内部的键值对中的键取出来作为类型（类似字符串的联合类型）
let 类型名 = keyof (类|接口|对象)
function add<T, U extends keyof Ixx>() {}

// Exclude,差集
type AB = 'a' | 'b'
type BC = 'b' | 'c'
type Demo = Exclude<AB, BC> // => type Demo = 'a'
    
// Pick，类型摘取
// 场景：已经声明的就类型，但是只想要其中的一部分
// 而且旧接口发生变化Pick的内容也变化（可以起到继承的效果）
type 类型名 = Pick<旧接口, "属性1" | "属性2">
type A = Pick<TState, "name" | "age">
interface A extends Pick<TState, "name" | "age">{// 此处放入更多类型声明}

// 常见结构 enum,interface,type
// 可以简单理解为用interface描述数据结构，用type描述类型关系
// 使用上的粗暴原则，接口能声明的就用接口，不行就用type
// type独有

// type可以被联合类型和元组赋值，接口不行
type X = typeof a // a变量可以是任何类型，由typeof取得
type StringOrNumber = string | number;  
type Text = string | { text: string };  
type NameLookup = Dictionary<string, Person>;  
type Callback<T> = (data: T) => void;  
type Pair<T> = [T, T];  
type Coordinates = Pair<number>;  
type Tree<T> = T | { left: Tree<T>, right: Tree<T> };


// 接口独有
// 同名接口自动合并
interface A {
    name:string;
}

interface A {
    age:number;
}

// 使用上被自动合并为
interface A {
    name:string;
    age:number;
}




// 接口变量修饰符
readonly 变量:类型 // 效果与const一致，基本数据类型不可修改，引用数据类型可以
// 区别是const用在变量声明，readonly是用于修饰接口内的类型
变量?:类型 // 可选

// 表达式
/// 联合类型（这是接口做不到，而类型别名做得到的）
类型1 | 类型2 | 类型3
type S = "big" | "mid" | "small" // 指定类型为S，即只能是里面的指定的字符串或者类型(接口的联合)
// 交叉类型
类型1 & 类型2 & 类型3
type S = interfaceA & interfaceB // 类型S需要满足接口A和接口B


// 新式语法，ES标准也跟着出了
?. // 可选链，对象的一直点语法需要判断里面是否包含特定变量不然容易报错，这个可选链自动中断

// class 的修饰符
public // 默认修饰符
private // 只有当前类能访问
protected // 子类或者当前类能访问

// 抽象类，通常作为父类，强制子类重写内部方法
abstract class A {}

// 泛型，用于使用时才确定类型的声明方式，场景：如写了一个参数是number和string的通用方法
// 类、函数、接口等都可以使用泛型，都是  函数名<> 类名<> 接口名<>
// 声明
function add<T>(参数:T):T{}
// 多个泛型
function add<T,U>(){// 使用泛型}
// 泛型继承接口
function add<T extends Iadd>():T{}

// 使用
add<填入具体类型，如接口，type等等>()
add<number>(1)
    
// 生产中常见的类型
DOM Event等 // DOMDivElement MouseEvent等
React // React.FC<参数类型>
Vue // 都可以查阅官网获得

// d是declare，只要由声明文件在就可以跨文件代码提示，以及别人引入类型使用
// 类型声明文件index.d.ts
// 声明命名空间
declare namespace 空间名 { 类型1 }
// 获取类型的方式都可以使用空间名.类型的方式使用
declare namespace React { 
    type FC = xxx 
}    
// 使用  
React.FC<提前声明的接口>
    

```

> typescript的好处
>
> 1、提供类型检查，更安全
>
> 2、丰富了JavaScript代码提示和代码补全
>
> 3、社区提供工具，生成文档
>
> 4、避免前后端接口问题扯皮
>
> 坏处
>
> 1、代码更多，像java一样啰嗦
>
> 2、学习成本高

### basetype

#### 原始

`boolean` `number` `string` `symbol` `bigInt`

`undefined` `null`\(**注：这两二货是所有类型的子类型**\)

```typescript
// let a: number = undefined; // 会编译报错，但是vscode不会警告
// let e: string = null; 
let b: boolean = false;
let c: symbol = Symbol(1);
let d: bigint = BigInt(9007199254740991);
let d: bigint = 12n; // 属于es2020的且没有polyfill
```

#### 数组

`number[]`  传统语法

`Array<number>` 接口+泛型语法

```typescript
let nums1: number[] = [1, 2, 3, 4, 5]
let nums2: Array<number> = [1, 3, 4, 5, 6]
```

#### 元组（存放不同类型的数组）

`[string, number]` ：第一个元素必须是`string` 第二个元素必须是`number`，且不能越界（新版）

不能少写，也不能多些

```typescript
// compile before
let tuple: [string, number] = ["test", 3] 

// compile after
let tuple = ["test", 3];
```

#### 枚举（enum）

> 使用场景：会员等级、星期、月份、颜色、上下左右等有限的内容

`enum` 的声明方式与`js`的对象类似

```typescript
enum Color{
    GREEN,// 中间以逗号隔开 ，默认索引从 0开始，也可以自定义索引
    RED,
}

console.log(Color.GREEN, Color.RED) // 0 1

```

枚举可以手动初始化，**一旦初始化之后**就需要全部初始化，不然会被警告

`enum`是 `number|string` 除此之外的能声明，但是不能使用，error：xx不能作为索引

```typescript
// compile before
enum Color {
    Red = 1,
    Green = '2',
    Blue = 4
}

// compile after
var Color;
(function (Color) {
    Color[Color["Red"] = 1] = "Red";
    Color["Green"] = "2";
    Color[Color["Blue"] = 4] = "Blue";
})(Color || (Color = {}));

// { '1': 'Red', '4': 'Blue', Red: 1, Green: '2', Blue: 4 }
即可以通过
    Color.['1'] 获取Red 也可以通过 Color["Red"] 获取 1
```

#### any

`any` 当不指定类型时的默认值就是any，我认为`any`就是`typescript`向`JavaScript`中和的写法。

```text
新的typescript项目不建议使用，可以在编译配置中警告这种行为
js向ts过渡的项目则难以避免【据说是降低人力消耗】
```

#### Object

与`any`类似，但是不能随意调用方法，类似于使用 `Object.create()` 创建

```javascript
function create(obj: object) {

}

create([])
create({})
create(function(){})
// create(1) // Argument of type 'number' is not assignable to parameter of type 'object'.
```

#### unknow

> 中和手段

```typescript
// compile before
let notSure: unknown = 4;
notSure = "maybe a string instead";
notSure = false;

// compile after
let notSure = 4;
notSure = "maybe a string instead";
notSure = false;
```

#### void

`void` 类型只能有两种值 `undefined` 和 `null`

```typescript
let test: void = undefined// 而undefined就是JavaScript函数的默认返回值
```

#### never

只能用作类型，用于函数 `return error` **不声明也会被推断**和 `死循环`，含义是函数永远不会有返回值

```javascript
function error(message: string): never {
    throw new Error(message);
}
```

#### assert类型断言

给any类型的内容确定类型（**在编译之前**）

`<string>any`

`any as string`

```typescript
let str: any = "str"
// 括号是提高优先级
let strLength: number = (str as string).length
let strLength2: number = (<string>str).length

const newStr = str as string
const len = newStr.length
```

```typescript
function getLength2(input: string | number): number {
  if (typeof input === 'string') {
    return input.length
  } else {
    return input.toString().length
  }
}
```

#### 超出接口限制的数据

>  **注：**你当然也可以修改接口

```JavaScript
interface School {
    name: string;
    address: string;
    age: number
}

let school = ({
    name: 'test',
    age: 11,
    address: 'local',
    email: 111 // 超出限制的数据，可以使用类型断言，
}) as School
```

#### 初探函数返回值

```typescript
function test(): number {
    // return 'zoulam';// error
    // return undefined;// ok, undefined 是number的子类型，但是要显式返回
    return 1;
}
```

#### 联合（宽松）（\|）类型

>  限制类型范围但是又不想使用`any`

```typescript
let h: number | string = 'kk'
h = 8;
```

#### 交叉类型（&）

> 必须满足两个类型或者说是接口，少一个多一个都不行，使用场景是对已有的代码添加新的属性限制，其实跟接口的 `extends`类似。

```typescript
let h = xx & bb
必须满足 xx 和 bb的类型

interface A {
    age: number;
    sayName: (name: string) => void
}
interface B {
    sayGender: (gender: string) => void
}

let a: A & B = {
    age: 18,
    sayName: (name) => { console.log(name); },
    sayGender: (gender) => { console.log(gender); }
}

a.sayGender('18')
```

### **interface**

>  ​	统一概念：接口就是描述对象形状的东西。
>
>  用于规范和约定大量的类型约束，且需要**重复使用**，如：大量函数有相同的**参数和返回值**常常用于处理相同数据
>
>  包括，**构造器，函数返回值**。
>
>  描述一类事物的特征，如人一定要具备如下属性：年龄、性别、身高、体重等
>
>  命名通常是 大写的 `I`+数据的名字

#### 规范数据类型

**const（变量）和readonly（属性）**

>  `readonly` 是属性，`const`变量，
>
>  共同点都是只能限制基本数据类型，但不能限制引用数据类型
>
>  对象里无法使用`const`语法，`readonly`很好的解决了这个问题

```typescript
interface People {
    // 虽然使用逗号结尾不会报错，但是建议使用分号
    // 必须实现
    name: string;
    // 可选
    age?: number;
    // 只读(也是必须实现的)，即只能赋值不能修改
    readonly isBig: boolean;
}

let people: People = {
    name: 'zoulam',
    isBig: true,
}

// people.isBig = false;// error
```

```typescript
let grade: ReadonlyArray<number> = [1, 2, 3, 4,];// 声明只读数组
// grade[1] = 5; // error Readonly比const更严格

const level = [1, 2, 3, 4]
// item 可修改
level[1] = 5;
```

#### 又见函数返回值

> 我的理解是声明了匿名接口

```typescript
function test(): { color: string; height: number } {
    return {
        color: 'green',
        height: 15
    }
}
```

**接口与函数**

> 爽的地方：
>
> 1、可以复用
>
> 2、使用时不用声明类型也带检查

```typescript
interface GetNewName {
    (firstName: string, secondName: string): string;
}

let getName: GetNewName;
getName = (firstName: string, secondName: string) => {
    return firstName + secondName;
}

let getChildren: GetNewName;
getChildren = (firstName, secondName) => {
    return firstName + secondName;
}
```

#### 接口与可索引类型

> 注：索引只能是两种类型 `string|number`，接口的扩展，也就是说满足keyvalue即可，不会限制数量。

```typescript
interface StringArray {
    // 索引为数字类型，内容为字符串
    [index: number]: string;
}

let myArray: StringArray = ["Bob", "Fred"];
```

**声明一致**

```typescript
interface test2 {
    // 后续声明的必须与返回值相同,此处必须是number,同时还能设置为只读    
    readonly [index: string]: number;
    age: number;
    // name: string;     // error
}

let t: test2 = {
    '1': 15,
    age: 16,
}
```

#### 类与接口（implements）

> 一个类可以实现多个接口

```typescript
interface A {
    // 规定this值
    curtime: Date;
    // 规定函数
    getHour(h: number): number;
}

interface B {
    // 规定this值
    name: string;
}

class GetTime implements A, B {
    curtime: Date;
    name: string;
    getHour(h) {
        return h;
    }
}
```

**规范构造器（暂时没搞清楚）**

```typescript
// 规范构造函数
interface GetNameConstructor {
    new(h: number, m: number): GetNameStatic;
}

// 规范实例部分
interface GetNameStatic {
    // 规范this.name
    name: string;
    // 规范函数
    getName(): string;
}

// 中间函数创建使用
function createName(a: GetNameConstructor, h: number, m: number): GetNameStatic {
    return new a(h, m)
}

class GetName implements GetNameStatic{
    name: string;
    constructor(h: number, m: number) {

    }
    getName() {
        return 'zoulam';
    }
}

let go: GetNameStatic = createName(GetName, 12, 5);
```

#### 接口继承接口（只能扩展）

**宽松接口&lt;interfaceName&gt;，可以继承多个**

```typescript
interface Shape {
    color: string;
    name: any;
}

interface PenStroke {
    penWidth: number;
}

interface Square extends Shape, PenStroke {
    // color: number; // error，不能修改原接口的类型
    name: string;// 重写只能是重写any
    sideLength: number;
}

// 宽松实现
let square = <Square>{};
square.color = "blue";
console.log(Object.prototype.toString.call(square));
// 强制实现
let qu: Square = {
    name: 'zoulam',
    color: 'green',
    penWidth: 1080,
    sideLength: 1920,
}
```

#### 混合类型

> 一个函数既可以是函数也可以是对象

```typescript
interface Counter {
    (start: number): string;
    interval: number;
    reset(): void;
}

function getCounter(): Counter {
    // let counter = <Counter>function (start: number) { };
    let counter = function (start: number) { } as Counter;
    counter.interval = 123;
    counter.reset = function () { };
    return counter;
}

let m = getCounter();
m(10);
m.reset();
m.interval = 5.0;
```

#### 接口继承类

> 接口会继承类的成员（包含`protected` 和 `private`）
>
> 使用限制：
>
>  1、当接口继承的类包含`private`内容时，只有用**他的子类**才能实现这个继承过的接口
>
>  2、无任何 `private`内容时就能，随意实现接口

```typescript
class Control {
    private state: any;
    public gogo: any;
}

interface SelectableControl extends Control {
    select(): void;
    // state:string; // error
    gogo: string;
}

class Button extends Control implements SelectableControl {
    select() { } // 一定要实现
    // gogo: number = 15 // error,即使不实现gogo也没所谓，因为已经从Control那里继承过来了
}

// error
// class I implements SelectableControl {
//     select() { }
// }
```

### 继承和实现速记

> 接口继承类，类继承类，接口继承接口，类实现接口

# 函数

```javascript
function add(a: string, b: string): string {
    return a + b
}

add('a', 'b')
```

```javascript
let add = (a: string, b: string): string => a + b; 
// hover到add上编译器能推断出他的类型：let add: (a: string, b: string) => string
```

```javascript
// 类型别名用箭头，接口用冒号
type fnType = (value1: string, value2: string) => string;
interface Ifn {
    (a: string, b: string): string
}
let add: fnType = (a: string, b: string): string => a + b;
let add2: Ifn = (a: string, b: string): string => a + b;
```

#### type vs interface

>  接口可以继承和扩展，可以被类实现（ `implement`）
>
> 类型别名更适合使用联合类型
>
> ​	`type buttonType = "large" | "small" | "middle"`

## Pick

```
interface TState {
	name: string;
	age: number;
	like: string[];
}
// 如果我只想要name和age怎么办，最粗暴的就是直接再定义一个（我之前就是这么搞得）
interface TSingleState {
	name: string;
	age: number;
}
// 这样的弊端是什么？就是在Tstate发生改变的时候，TSingleState并不会跟着一起改变，所以应该这么写
interface TSingleState extends Pick<TState, "name" | "age"> {};
```

## **内置类型**

> 即`typescript`写好的接口

```typescript
function add(arguments) {
    let nums: IArguments = arguments
}

const date: Date = new Date()
let body: HTMLElement = document.body
```

