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

## basetype

### 原始

`boolean` `number` `string` `symbol`

`unddefined` `null`\(**注：这两二货是所有类型的子类型**\)

```typescript
let a: number = undefined;
let e: number = null;
```

### 数组

数组

`number[]` `Array<number>`

### 元组（存放不同类型的数组）

元组 tuple

`[string, number]` ：第一个元素必须是`string` 第二个元素必须是`number`，且不能越界（新版）

不能少写，也不能多些

### 枚举（enum）

> 使用场景：会员等级、星期、月份、颜色、上下左右等有限的内容

`enum` 与 `class` 的声明方式类似

枚举类的 `value` 是 `undefined`

`enum`是 `number|string` 除此之外的能声明，但是不能使用，error：xx不能作为索引

```typescript
enum Color {
    Red = 1,
    Green = '2',
    Blue = 4
}
let c: Color = Color.Green;
let g: Color = Color.Red;
let d = Color['2'];
console.log(typeof c);// string
console.log(typeof g);// number
console.log(Object.prototype.toString.call(d));// undefined
```

### any

`any` 当不指定类型时的默认值就是any，我认为any就是JavaScript的写法。

### Object

与`any`类似，但是不能随意调用方法，类似于使用 `Object.create()` 创建

### void

`void` 类新只能由两种值 `undefined` 和 `null`

### never

只能用作类型，用于函数 `return error` **不声明也会被推断**和 `死循环`

### assert

给any类型的内容确定类型（**在编译之前**）

`<string>any`

`any as string`

### 初探函数返回值

```typescript
function test(): number {
    // return 'zoulam';// error
    // return undefined;// ok, undefined 是number的子类型，但是要显式返回
    return 1;
}
```

### 联合（宽松）（\|）类型

```typescript
let h: number | string = 'kk'
h = 8;
```

### 交叉类型（&）

```text
let h = xx & bb
必须满足 xx 和bb的类型
```

## interface

> 用于规范和约定大量的类型约束，且需要**重复使用**，如：大量函数有相同的**参数和返回值**常常用于处理相同数据
>
> 包括，**构造器，函数返回值**

### 规范数据类型

#### const（变量）和readonly（属性）

```typescript
interface People {
    // 虽然使用逗号结尾不会报错，但是建议使用分号
    // 必须实现
    name: string;
    // 可选
    age?: number;
    // 只读(也是必须实现的)
    readonly isBig: boolean;
}

let people: People = {
    name: 'zoulam',
    isBig: true,
}

// people.isBig = false;// error

let grade: ReadonlyArray<number> = [1, 2, 3, 4,];// 声明只读数组
// grade[1] = 5; // error Readonly比const更严格

const level = [1, 2, 3, 4]
// item 可修改
level[1] = 5;
```

### 又见函数返回值

> 我的理解是声明了匿名接口

```typescript
function test(): { color: string; height: number } {
    return {
        color: 'green',
        height: 15
    }
}
```

#### 接口与函数

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

### 接口与可索引类型

> 注：索引只能是两种类型 `string|number`

```typescript
interface StringArray {
    // 索引为数字类型，内容为字符串
    [index: number]: string;
}

let myArray: StringArray;
myArray = ["Bob", "Fred"];
```

#### 声明一致

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

### 类与接口（implements）

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
    constructor(h: number, m: number) {
        this.curtime;
    }
    getHour(h) {
        return h;
    }
}
```

#### 规范构造器（**暂时没搞清楚**）

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

### 接口继承接口（只能扩展）

#### 宽松接口`<interfaceName>`

```typescript
interface Shape {
    color: string;
    name: any;
}

interface PenStroke {
    penWidth: number;
}

interface Square extends Shape, PenStroke {
    // color: number; // error
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

### 混合类型（**不懂**）

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

### 接口继承类

> 接口会继承类的成员（包含`protected` 和 `private`）
>
> 使用限制：1、当接口继承的类包含`private`内容时，只有用**他的子类**才能实现接口
>
> 2、无任何 `private`内容时就能，随意实现接口

```typescript
class Control {
    private state: any;
}

interface SelectableControl extends Control {
    select(): void;
}

class Button extends Control implements SelectableControl {
    select() { }
}

// error
// class I implements SelectableControl {
//     select() { }
// }
```

