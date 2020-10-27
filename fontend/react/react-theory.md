---
description: dom-diff……
---

# \[react\]-theory

## \[react\]-theory

## dom-diff

## setState

**禁止**使用赋值的方式修改 `this.state.val`

[setState什么时候是异步的](https://zh-hans.reactjs.org/docs/faq-state.html#when-is-setstate-asynchronous)

[深入学习SetState](https://github.com/sisterAn/blog/issues/26)

同步：

​ 1、参数传入回调函数，用回调函数的返回值作为参数 **【闭包】**

​ 2、不通过`React`的 `addEventListener`

异步：

​ 1、默认情况下就是异步【多次操作会被合并成一次操作】

​ 2、父组件和子组件都使用react事件（如：`onClick`）调用 `setState` 修改是同步的，而不是子组件被渲染两次**【提高性能】**

`this.state` 和 `this.props【函数的形参，类的静态属性】` 与我们在页面看到的值是一致的

```text
import React, { Component } from 'react'

export default class State extends Component {
    constructor() {
        super()
        this.state = {
            count: 0,
            val: 0
        }
    }

    // !异步
    handle() {
        this.setState({ val: this.state.val + 1 })
        this.setState({ val: this.state.val + 1 })
        this.setState({ val: this.state.val + 1 })
    }

    // ! 同步
    incrementCount() {
        this.setState((state) => {
            return { count: state.count + 1 }
        });
    }

    handleSomething() {
        this.incrementCount();
        this.incrementCount();
        this.incrementCount();
    }
    render() {

        return (
            <div>
                {/* 每次都+3 */}
                同步：{this.state.count}
                {/* 每次+1 */}
                异步：{this.state.val}
                <button onClick={() => { this.handleSomething(); this.handle() }}>change</button>
            </div>
        )
    }
}
```

### 面试题

输出是 0 0 2 3

```text
class Example extends React.Component {
  constructor() {
    super();
    this.state = {
      val: 0
    };
  }

  componentDidMount() {
    this.setState({val: this.state.val + 1});
    console.log(this.state.val);    // 第 1 次 log

    this.setState({val: this.state.val + 1});
    console.log(this.state.val);    // 第 2 次 log

    setTimeout(() => {
      this.setState({val: this.state.val + 1});
      console.log(this.state.val);  // 第 3 次 log

      this.setState({val: this.state.val + 1});
      console.log(this.state.val);  // 第 4 次 log
    }, 0);
  }

  render() {
    return null;
  }
};
```

