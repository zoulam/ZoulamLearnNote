# AST抽象语法树

每一层都包含的信息

`type` `start` 在抽象语法树的起始位置`end` 在AST终点位置

### 拆解的第一层

`Program`

​	包含以下信息

​	代码 `body`		`sourceType`

### 拆解的第二层（即代码实体）

`body`

​	下面是各种各种 `XXXDeclaration`

​	声明下面就是各种代码的拆解

## 下面以这段代码分析AST

```javascript
async function add(a, b) {
    let c = a + b;
    return c;
}
```

![第一步拆解](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200924102801144.png)

> 上面的`generator` 的意思是否是生成器函数的意思 `function * gen(){}`就为true



拆解为`params` 和 `body` 下面分析`body`

`body`被分解为

​	 `BlockStatement` 完成函数详细拆解

​	`ReturnStatement`