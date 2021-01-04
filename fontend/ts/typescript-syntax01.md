# \[typescript\]syntax01

> typescript的好处
>
> 1、提供类型检查，更安全
>
> 2、丰富了JavaScript代码提示和代码补全
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
let a: number = undefined;
let e: string = null;
```

#### 数组

`number[]` `Array<number>`

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
    xx,// 中间以逗号隔开 ，默认索引从 1开始，也可以自定义索引
}
```

枚举类的 `value` 是 `undefined`

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

`void` 类新只能由两种值 `undefined` 和 `null`

```typescript
let test: void = undefined// 而undefined就是JavaScript函数的默认返回值
```

#### never

只能用作类型，用于函数 `return error` **不声明也会被推断**和 `死循环`，含义是函数永远不会有返回值

```text
function error(message: string): never {
    throw new Error(message);
}
```

#### assert

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

> 必须满足两个类型或者说是接口，少一个多一个都不行，使用场景是对已有的代码添加新的属性限制

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

>  用于规范和约定大量的类型约束，且需要**重复使用**，如：大量函数有相同的**参数和返回值**常常用于处理相同数据
>
> 包括，**构造器，函数返回值**。
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

> 注：索引只能是两种类型 `string|number`

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

## **内置类型**

> 即`typescript`写好的接口

```typescript
function add(arguments) {
    let nums: IArguments = arguments
}

const date: Date = new Date()
let body: HTMLElement = document.body
```

