---
description: dom-diff……
---

# \[react\]-theory

## dom-diff

1、只比较同层级的节点

2、给节点添加 `key` 作为标识，用于判断下一次是否需要重新渲染

3、跨层级的移动视为**删除和新增**

```text
// 1、单节点diff
```

## setState

**禁止**使用赋值的方式修改 `this.state.val`

[setState什么时候是异步的](https://zh-hans.reactjs.org/docs/faq-state.html#when-is-setstate-asynchronous)

[深入学习SetState](https://github.com/sisterAn/blog/issues/26)

同步：

1、参数传入回调函数，用回调函数的返回值作为参数 **【闭包】**

2、在`React`监控外的操作，如 `setTimeout` 和 `setInterval`

异步：

1、默认情况下就是

`this.state` 和 `this.props【函数的形参，类的静态属性】` 与我们在页面看到的值是一致的

```text
输出情况
异步1 2 3 4 ……
同步1 4 7 10 ……
react监听外1 4 7 10 ……
```

```javascript
import React, { Component } from 'react'

export default class SetState extends Component {
    state = { val: 1, cbVal: 1, noListener: 1 }

    // 异步，三次操作合并
    syncTest() {
        this.setState({ val: this.state.val + 1 })
        this.setState({ val: this.state.val + 1 })
        this.setState({ val: this.state.val + 1 })
    }

    // 同步
    noReactListener() {
        setTimeout(() => {
            this.setState({ noListener: this.state.noListener + 1 })
            this.setState({ noListener: this.state.noListener + 1 })
            this.setState({ noListener: this.state.noListener + 1 })
        }, 0)
    }

    // 同步
    changeVal = () => {
        this.setState((state) => {
            return { cbVal: state.cbVal + 1 }
        })
    }

    handleChange = () => {
        this.changeVal()
        this.changeVal()
        this.changeVal()
    }

    render() {
        return (
            <div>
                异步{this.state.val}<br/>
                同步{this.state.cbVal}<br/>
                react监听外{this.state.noListener}<br/>
                <br/>
                <button onClick={() => { this.handleChange(); this.syncTest(); this.noReactListener() }}>add</button>
            </div>
        )
    }
}
```

### 面试题

```javascript
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

**输出是** `0 0 2 3`

