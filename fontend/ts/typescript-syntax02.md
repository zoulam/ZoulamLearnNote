# \[typescript\]syntax02

## class

### 修饰符

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

### 抽象类

### 继承

#### 类继承类

> 继承了除了 `private`之外的全部内容

## 泛型

### 1、为什么要泛型：

# 文件声明

`JQuery.d.ts`

不用自己手写

[DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped)

[搜索](https://www.typescriptlang.org/dt/search/)

@Types

