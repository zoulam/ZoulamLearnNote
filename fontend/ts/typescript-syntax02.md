# \[typescript\]syntax02

### class

#### 语法糖

> 只要加上修饰符就可以直接将参数挂载到`this`上

```typescript
// complie before
class Info {
    constructor(public name: string, private age: number) {

    }
}
// complie after
class Info {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
}
```

#### 修饰符

|  |  |
| :--- | :--- |
| `public` | 默认修饰符，可省略 |
| `protected` | 外部不可取用，子类通过内部方法调用 |
| `private` | 外部不可取用 |
| `get`（存取器） | 用 `defineProperty`实现，声明时是函数格式，使用时当作变量，暴露类的内部属性 |
| `set` | 用 `defineProperty`实现，声明时是函数格式，使用时当作变量赋值 |
| `static` | 不会被实例化的属性，与`private`相比 `static` 可以使用 `类名.属性`的方式调用，原理是`static`是不会被实例化的内容 |
| `readyonly` | 只能在声明时，或者类中初始化指定内容  `this` `构造器参数` |

```typescript
class Animal1 {
    readonly age: number;
    // 初始化方式1
    readonly size: number = 18;
    constructor(readonly name: string = '1', size: number) {
        // 初始化方式2
        this.size = size;
    }
}
```

```typescript
// compile before
class A {
    protected value1: number = 5
    private value2: string = "str"
    static value3: number = 18
    constructor() {
        this.value = 18
    }
    get value() {
        return A.value3
    }
    set value(newVal) {
        this.value = newVal
    }
}

// compiler after
var A = (function () {
    function A() {
        this.value1 = 5;
        this.value2 = "str";
    }
    Object.defineProperty(A.prototype, "value", {
        get: function () {
            return A.value3;
        },
        enumerable: false,
        configurable: true
    });
    A.value3 = 18;
    return A;
}());
```

#### 抽象类

> 与接口不同的是抽象类包含了更多的实现细节，常用作基类（祖宗类）

```typescript
abstract class Components{

}
```

#### 继承

**类继承类**

> 继承了除了 `private`之外的全部内容，即可以直接使用`this`获取

### 泛型

#### 1、为什么要泛型：

>  泛型可以在实际运行中才确定类型，而不是过早的写死。
>
>  `T`表示一个不确定的类型，使用时才确定，确定之后需要做到前后一致 ，如开始用了 `string`，后面 `T`就是`string`。
>
>  类型可以自动推断

```JavaScript
function createArray<T>(len: number, value: T): T[] {
    let res = []
    for (let i = 0; i < len; i++) {
        res.push(value)
    }
    return res
}


let arr = createArray<string>(3, 'test')
let arr2 = createArray(3, 12) // 此处也会推断出arr2的元素是number
```



```typescript
function echo<T>(arg: T): T {
    return arg
}
const result1 = echo(123)


// 只能交换number 和string，且一定要按顺序，传入其他类型就报错
function swap(tuple: [number, string]): [string,number] {
    return [tuple[1], tuple[0]]
}

// 泛型写法
function swap<T, U>(tuple: [T, U]): [U, T] {
    return [tuple[1], tuple[0]]
}

const result2 = swap(['string', 123])
const result2 = swap([false, 123])
```

#### 2、可以继承

```typescript
interface People {
    name: string;
    age: number;
}

function getInfo<T extends People>(arg: T) {
    return arg
}

console.log(getInfo({ name: 'lala', age: 18, momolength: 'go' }))
```

#### 3、泛型与类

```typescript
class Queue<T> {
  private data = [];
  push(item: T) {
    return this.data.push(item)
  }
  pop(): T {
    return this.data.shift()
  }
}
const queue = new Queue<number>()
```

#### 4、泛型与接口

```typescript
interface KeyPair<T, U> {
  key: T;
  value: U;
}

let kp1: KeyPair<number, string> = { key: 1, value: "str"}
let kp2: KeyPair<string, number> = { key: "str", value: 123}
```

### 类型别名

```typescript
type superString = 'lala' | 'lulu' | 'momo' // superString是一个类型，只能使用限制的字符串赋值
type numString = number | string // numString类型可以是number或者string
```

### 文件声明

`xx.d.ts`:ts的代码补全只在当前文件下生效，要想其他文件有补全提示就需要使用声明

```typescript
declare const result2: [number, string];
```

部分文件的提示不能生效，可以在`tsconfig.json`下添加

```
{
	"include":["*/**"], /* 编译文件夹下的所有文件*/
}
```

[DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped)

[搜索](https://www.typescriptlang.org/dt/search/)

### [工具类型](https://www.typescriptlang.org/docs/handbook/utility-types.html)

```typescript
interface IPerson {
  name: string
  age: number
}

let viking: IPerson = { name: 'momo', age: 20 }
// 全部转化为可选类型
type IPartial = Partial<IPerson>
let viking2: IPartial = { }

// Omit，它返回的类型可以忽略传入类型的某个属性

type IOmit = Omit<IPerson, 'name'>
let viking3: IOmit = { age: 20 }
```

| target表示处理的接口 |  |
| :--- | :--- |
| `Readonly<target>` | 全部设置为只读 |
| `Record<key,value>` | 强制约束内部键值对 |
|  |  |
|  |  |
|  |  |
|  |  |
|  |  |
|  |  |
|  |  |

```typescript
interface PageInfo {
    title: string;
}

type Page = "home" | "about" | "contact" | "go" | "fuck";

// <key,value> key只能是string|number|symbol，下移见括号内的内容调换会报错
const nav: Record<Page, PageInfo> = {
    // about、是Page的类型，
    // title是PageInfo的类型,title必须实现但不一定在Page的范围
    about: { title: "g" },
    contact: { title: "contact" },
    home: { title: "home" },
    go: { title: "home" },
    fuck: { title: "fuck" } // 如果将本行注释掉会提示缺少 fuck
};

console.log(nav.about);
```

## 关于树形组件的接口设计

[原回答](https://www.zhihu.com/question/274940977/answer/383016528)

这是基于原生`JavaScript`的逐渐变成`React`的样子

```typescript
interface Node {
    textContent: string;
    child: Node[]
}
```

```typescript
// 节点类型需要确定
interface Node {
    type: 'leaf' | 'tree'
    textContent: string
    children?: Node[]
}
```

```typescript
// 后续的字段单独添加到props上维护
interface Node {
    type: 'leaf' | 'tree'
    props: {
        textContent: string
        children?: Node[]
        selected: boolean
        disabled: boolean
    }
}
```

```typescript
// 类型不一定是那么简单，还可以自定义渲染模板
type typeStr = 'leaf' | 'tree'
interface Node {
    type: typeStr | Function
    props: {
        textContent: string
        children?: Node[]
        selected: boolean
        disabled: boolean
    }
}
```

