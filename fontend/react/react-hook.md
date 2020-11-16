# \[react\]hook

# hookAPI使用

## 1、useState

>  函数参数是state的初始值，返回值是数组，元素1是当前state、元素2是修改该state的函数

```javascript
const [state, setState] = useState(0)
```

## 2、useEffect

> 传入两个参数
>
> 参数1是回调函数，回调函数的返回值用于清除副作用
>
> 参数2是监听的数组，当参数二为空每次`render`都会执行一次，传入空数组则是**仅在**页面第一次渲染时执行，后续不再执行。
>
> ​	类似于`classComponent`的两个生命周期
>
> ​	`componentDidMount` 添加副作用
>
> ​	`componentWillUnmount` 清除副作用

```javascript
import React, { useState, useEffect } from 'react';

export default function TestHooks() {
    const [date, setDate] = useState(new Date());
    useEffect(() => {
        const timer = setInterval(() => {
            setDate(new Date())
        }, 1000)
        return () => clearInterval(timer)
    }, [date])

    return (
        <div>
            {date.toLocaleTimeString()}
        </div>
    )
}
```

## 3、useContext

> 使用处理 contextType 和 Consumer之外的方式获取context

<img src="https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201116154119292.png" alt="getcontext" style="zoom:67%;" />

```JavaScript
import React, { useContext } from 'react';
const MyContext = React.createContext({ val: 15 })
export default function Context() {
    const contextValue = { val: 17 }
    return (
        <MyContext.Provider value={contextValue}>
            <TestHooks/>
        </MyContext.Provider>
    )
}

function TestHooks() {
    const value = useContext(MyContext)
    return (
        <div>
            {value.val}
            {/* 17 */}
        </div>
    )
}
```

## 4、useReducer

>  类似于`redux`修改state的方式，是更强大的 `useState`的替代品
>
> 传入三个参数
>
> <img src="https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201116155150679.png" alt="three args" style="zoom:67%;" />
>
> ​	参数1： reducer
>
> ​			使用时只需要传入`action`，而不同传入`state`。
>
> ​	参数2： 初始值
>
> ​	参数3 ：回调函数，可以用于恢复初始值

```javascript
import React, { useReducer } from 'react';

function init(initialCount) {
    return { count: initialCount };
}

function reducer(state, action) {
    switch (action.type) {
        case 'increment':
            return { count: state.count + 1 };
        case 'decrement':
            return { count: state.count - 1 };
        case 'reset':
            return init(action.payload);
        default:
            throw new Error();
    }
}

export default function TestHooks({ initialCount }) {
    const [state, dispatch] = useReducer(reducer, initialCount, init);
    return (
        <>
            Count: {state.count}
            <button
                onClick={() => dispatch({ type: 'reset', payload: initialCount })}>
                Reset
        </button>
            <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
            <button onClick={() => dispatch({ type: 'increment' })}>+</button>
        </>
    );
}
```

## 5、useCallback（用于优化）

>  传入两个参数
>
> ​	参数1：回调函数，执行消耗大量性能的操作
>
> ​	参数2：监听值数组（**依赖项**），监听值发生变化才执行
>
> 返回值
>
> ​	memo函数

```javascript
import React, { useCallback, useState } from 'react';


export default function TestHooks() {
    let [a, setA] = useState(1)
    let [b, setB] = useState(2)
    let [c, setC] = useState(0)
    function add(a, b) {
        return a + b
    }
    const addCallback = useCallback(() => {
        // 假设此处进行很耗费性能的操作
        setC(add(a, b))
    }, [a, b])

    return (
        <>
            {a}<br />
            {b}<br />
            {c}

            <button onClick={() => { setA(a + 1); addCallback() }}>adda</button>
            <button onClick={() => { setB(b + 1); addCallback() }}>addb</button>
        </>
    );
}
```



## 6、useMemo（用于优化）

>  传入两个参数
>
> ​	参数1：回调函数，执行消耗大量性能的操作
>
> ​	参数2：监听值数组（**依赖项**），监听值发生变化才执行
>
> 返回值
>
> ​	memo值 【效果是自动执行了上面`useCallback`函数】

```javascript
import React, { useMemo, useState } from 'react';

export default function TestHooks() {
    let [a, setA] = useState(1)
    let [b, setB] = useState(2)
    function add() {
        return a + b
    }
    const memoValue = useMemo(() => add(a, b), [a, b])

    return (
        <>
            {a}<br />
            {b}<br />
            {memoValue ? memoValue : 1}<br />
            <button onClick={() => { setA(a + 1) }}>adda</button>
            <button onClick={() => { setB(b + 1) }}>addb</button>
        </>
    );
}
```

## 7、useRef

> 场景：当节点层数深难以获取，可以使用`ref.current`获取
>
> ​	1、点击聚焦，展开
>
> ​	2、点击外部失去焦点，折叠
>
> ref值发生变化不会触发重新渲染页面。
>
> ​	useRef会在每次渲染时返回同一个ref对象，即返回的ref对象在组件的整个生命周期内保持不变。自建对象每次渲染时都建立一个新的。

```javascript
import React, { useRef } from 'react';

export default function TestHooks() {
    const inputEl = useRef(null)
    // 点击按钮聚焦并且将原来的红色改为蓝色
    const onButtonClick = () => {
        inputEl.current.attributes.style.value = "color:blue"
        inputEl.current.focus()
    }
    return (
        <>
            <input ref={inputEl} type="text" style={{ color: "red" }} />
            <button onClick={onButtonClick}>点击聚焦</button>
        </>
    );
}
```

```javascript
// 回调型useRef
const overlayViewRef = useRef(null)
<Overlay.View
	 ref={v => overlayViewRef = v}
	>
	</Overlay.View>
	
overlayViewRef.current.close()	
```

> ​	关于一整个声明周期都不会变的问题，普通对象和ref值的变化都不会触发render，但是两者都能被修改，所以手动render的适合普通对象发生了变化，而ref值并不会变化后渲染到页面上。

<img src="https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201116171106238.png" alt="都改变了值" style="zoom:67%;" />



<img src="https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201116171138484.png" alt="invoke rerender" style="zoom:67%;" />

```javascript
import React, { useRef, useState } from 'react';


export default function TestHooks() {
    const [state, setState] = useState(true)
    const inputEl = useRef({ sort: 8 })
    const value = {
        current: {
            sort: 8
        }
    }

    const hanldeChange = () => {
        value.current.sort++
        console.log('obj', value.current.sort);
        inputEl.current.sort++
        console.log('ref', inputEl.current.sort);
    }

    return (
        <>
            {inputEl.current.sort}<br />
            {value.current.sort}<br />
            <button onClick={() => { hanldeChange() }}>change ref and normal object</button>
            <button onClick={() => { setState(!state) }}>rerender</button>
        </>
    );
}
```



## 8、useImperativeHandle

> **Imperative**：必要的，不可避免的；紧急的；命令的，专横的；势在必行的；[语]祈使的。
>
> ​	搭配`forwarRef`使用，向父组件暴露`ref`

<img src="https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201116174007879.png" alt="flow" style="zoom:67%;" />



![error](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201116174123425.png)



显示是空但是获得了值

```javascript
import React, { useRef, useImperativeHandle, forwardRef, useState } from 'react';

function FancyInput(props, ref) {
    const inputRef = useRef();
    useImperativeHandle(ref, () => ({
        name: "expose more",
        focus: () => {
            inputRef.current.focus();
        }
    }));
    return <input ref={inputRef} />;
}

// 类似于高阶组件
FancyInput = forwardRef(FancyInput);

export default function Parent() {
    const el = useRef(null);
    return (
        <>
            <FancyInput ref={el} />
    		// el.current.focus 可能为空
            <button onClick={() => { el.current.focus && el.current.focus() }}>点击聚焦</button>
        </>
    );
}
```



## 自定义hooks

> 命名useXxx

### 是否点击点击节点外

```typescript
import { RefObject, useEffect } from "react";


/**
 * @description 点击节点外的内容执行handler
 * @param ref 监听的dom
 * @param handler 点击外部dom的处理，如关闭
 */

function useClickOutside(ref: RefObject<HTMLElement>, handler: Function) {
  useEffect(() => {
    const listener = (event: MouseEvent) => {
      if (!ref.current || ref.current.contains(event.target as HTMLElement)) {
        return
      }
      handler(event)
    }
    document.addEventListener('click', listener)
    return () => {
      document.removeEventListener('click', listener)
    }
  }, [ref, handler])
}

export default useClickOutside
```

### 函数防抖

```typescript
import { useState, useEffect } from 'react'

/**
 * @description 节流
 * @param value 监听值
 * @param delay
 */
function useDebounce(value: any, delay = 300) {
  const [debouncedValue, setDebouncedValue] = useState(value)
  useEffect(() => {
    const handler = window.setTimeout(() => {
      setDebouncedValue(value)
    }, delay)
    return () => {
      clearTimeout(handler)
    }
  }, [value, delay])
  return debouncedValue
}

export default useDebounce;
```

### 获取键盘点击

```text
import { useState, useEffect } from 'react'

const useKeyPress = (targetKeyCode) => {
  const [keyPressed, setKeyPressed] = useState(false)

  const keyDownHandler = ({ keyCode }) => {
    if(keyCode === targetKeyCode) {
      setKeyPressed(true)
    }
  }
  const keyUpHandler = ({ keyCode }) => {
    if(keyCode === targetKeyCode) {
      setKeyPressed(false)
    }
  }
  useEffect(() => {
    document.addEventListener('keydown', keyDownHandler)
    document.addEventListener('keyup', keyUpHandler)
    return () => {
      document.removeEventListener('keydown', keyDownHandler)
      document.removeEventListener('keyup', keyUpHandler)     
    }
  }, [])
  return keyPressed
}

export default useKeyPress
```

