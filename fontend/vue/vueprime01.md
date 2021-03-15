# \[vue\]语法遍历

## config

>  以官网为准，可以配置一下内容，他们都是挂载在 `vue.config`上的属性。
>
> ​	1、是否显示日志和警告
>
> ​	2、
>
> ​	3、是否允许 `vue-devtools`检查代码
>
> ​	4、自定警告或错误格式
>
> ​	5、忽略指定节点（指不渲染）
>
> ​	6、给 `v-onkeyup`自定义键位别名（将原来难以记忆的数字转为见名知意的字符串）
>
> ​	7、监控性能（初始化、编译、渲染和打补丁）用于为优化做出选择
>
> ​	8、是否关闭生产提示





> 本节是语法入门，并没有脱离语法层面，仅供快速复习。

## 语法入门

### 指令

#### `v-model` | `:`

**限制**：

* `<input>`
* `<select>`
* `<textarea>`
* components

**修饰符**：

* `.lazy`-取代 `input` 监听 `change` 事件
* `.number`- 输入字符串转为有效的数字
* `.trim`- 输入首尾空格过滤

#### `v-show`

`v-show` 是使用 `display: none;` **\(切换开销⽐较⼩，初始开销较⼤\)**

#### `v-if` `v-else-if` `v-else`

`v-if` 是 remove dom node 的操作，反复使用的时候性能差。**（初始渲染开销较⼩，切换开销⽐较⼤）**

#### `v-once`

> 只渲染元素一次，后续页面变化将其视为静态元素直接跳过
>
> `v-show`是使用 `display:none` 设置， `v-if` 则是操作DOM性能较差，且会存在缓存的问题，使用时可以使用
>
> `:key` 给元素命名（key必须是唯一的且确定）
>
>  **1、不能使用random，每次都被认为是不一样的key**
>
>  **2、不能用数组index，因为数组长度（除了pop）改变时大量的index会变化**

#### `v-for="(item, index) of list"`

> 当和 `v-if` 一起使用时，`v-for` 的优先级比 `v-if` 更高。即会出现`v-for`先渲染出列表，却被`v-show`隐藏导致性能浪费

#### `v-for="item in list"`

#### `v-bind`

> 简写 `:` 动态绑定数据

#### `v-on` | `@` 

> 简写 `@` 绑定事件

#### `v-html` `v-text`

```

```



#### `{{}}`

> 标签内包裹属性

#### `""`

> 当行内值是通过Vue绑定的动态值时， `"'string'"` 这样才能声明字符串，即`""`包裹的内容是`JS`表达式
>
> **注** 双引号和单引号需要配合使用

#### `v-slot`

### 标签

#### `<slot></slot>`

> 指声明在组件内的标签，用于读取组件内，包裹的子内容（**文本、标签、组件**）

**匿名插槽**

> 读取未命名组件的子标签

**具名插槽**

组件声明：

`<slot name="insert"></slot>`

在**slot**中添加默认插槽内容，在组件中没有子标签时生效

`<slot><div>show time</div></slot>`

组件使用：

`<div v-slot="insert">我被插入slot</div>`

**作用域插槽**

数据流向

组件模板 `content` =&gt; `props` =&gt; `props.content`

```javascript
    <div id="app">
        <child>
            <!-- 作用域插槽 -->
            <!-- slot-scope内传入的是一个对象，里面包含模板中传入的数据 -->
            <template slot-scope="props">
                <li>{{props.content}}</li>
            </template>
        </child>
    </div>
    <script>
        Vue.component('child', {
            props: ['content'],
            template: `<div>
                    <slot content="i am a info"></slot>
                </div>
            `
        })
        const vm = new Vue({
            el: '#app'
        });
    </script>
```

#### `<component :is="type"></component>`

`type` 可以在实例的`data`中配合 `methods` 动态设置数据，实现动态加载组件

#### `<template>`

一个不会被识别到页面实体，起包裹作用的标签

#### `<transition>`

用于设置**单个**元素的的过渡效果，实现方式（添加类名）

#### `<transition-group>`

设置**多个**元素的过渡效果，原理是给每一个标签单独设置一个`<transition></transition>`

### js写动画

> 配合**Vue**的钩子函数、 `setTimeout` API以及css的`transition`属性，实现使用纯JS写动画效果。

### 组件设计

#### 规范

> 组件声明
>
> ~~简单命名 `child` **\(开发中禁止使用\)**~~
>
> 短横线命名`kebab-case` 如：`list-item`
>
> 大驼峰`PascalCase` 如：`TodoItem`
>
> 组件使用
>
> 强制使用 `<todo-item><todo-item>`

#### 全局组件

> 挂载在Vue对象上，**缺陷：**使用打包工具时，即使是不使用的全局组件也会被打包到代码中
>
> **为什么组件中的数据一定是函数？**
>
> 因为组件使用时每一个组件之间的数据应该是相互独立的，函数声明可以使用闭包的方式创建安全独立的数据。

```javascript
        Vue.component('组件名', {
            props: [],
            template: `<div v-once>this is child one</div>`,
            data() {
                return {
                    value: 'zoulam'
                }
            },
            methods: {

            }
        })
```

#### `props`

> props可以是数组
>
> 也可以是对象，当是对象是可以设置部分功能

```javascript
        Vue.component('child', {
            props: {
                content: {
                    // 自定义的验证器 return false在控制台报错
                    validator: function (value) {
                        // if (value.length < 7) alert('账号过短')
                        return value.length > 5
                    },
                    type: String,
                    // 此处表明传入的数据只能是String或者Number类型（无顺序）
                    // type: [String, Number],
                    required: false,// true设置必须传入值，不然报错,默认值是false
                    default: 'this is default value'
                }
            },
            template: '<p>{{content}}</p>',
        })


        Vue.component('child2', {
            props: {
                content: String,
            template: '<p>{{content}}</p>',
        })
```

#### 局部组件

> 对象语法,使用时必须在父组件的 `options`的`components`下注册

```javascript
const BackHome = {
    props: ['content', 'index'],
    template: '<strong style="color:red"><li>局部注册组件：{{content}} <button @click="deleteItem()">x</button></li></strong>',
}
const vm = new Vue({
    el: '#app',
    components: {
        // gohome 对组件BackHome重命名
        gohome: BackHome
    }
})
```

#### 文件拆分写法

注册

```javascript
<template>
  <button :class="['my-btn',btnStyle]">
      <slot></slot>
  </button>
</template>

<script>
export default {
  props: {
    btnStyle: String,
    btnText: String,
  },
};
</script>
```

使用

```javascript
<template>
  <div id="app">
    <h1>父子组件通信</h1>
    <ParentComponent />
  </div>
</template>

<script>
import ParentComponent from "./ChildWithParent/parent1";
export default {
  name:'#app',
  components:{
    ParentComponent,
  }
};
</script>
```

### 属性

#### `ref`

> 子组件注册，父组件取用

注册：

`<div ref="hello" @click="showInfo">hello world</div>`

使用：

`this.$refs.hello`

#### `:key`

给标签添加`:key`作为标识，可以避免**Vue**对页面重新渲染出现复用（缓存）问题

### 对象实例属性

#### `$emit('事件名'， 事件参数) & :native`

> 自定义事件（**用于子组件**），使用方式是挂载到原有事件函数中
>
> `<child @click="show"></child>` 会被Vue认为是自定义事件
>
> `<child2 @click.native="show"></child2>` 需要添加属性才会被认为是原生事件

```javascript
        const BackHome = {
            props: ['content', 'index'],
            template: '<strong style="color:red"><li>局部注册组件：{{content}} <button @click="deleteItem()">x</button></li></strong>',
            methods: {
                deleteItem: function () {
                    // $emit 自定义事件 this.index从上方的props取值
                    this.$emit('delete', this.index)
                }
            }
        }
```

### 生命周期函数

> 偷张官方文档的图。
>
> 生命周期的简要解释：
>
> 可以认为生命周期函数就是在Vue的实例内容执行特定过程时会自动执行的函数，可以通过这种函数提供更丰富的功能实现。

![Vue &amp;\#x5B9E;&amp;\#x4F8B;&amp;\#x751F;&amp;\#x547D;&amp;\#x5468;&amp;\#x671F;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/lifecycle.png)

## 小技巧

### 内置三剑客`computed` `methods` `watch`

`computed`（计算属性） `methods` `watch`（侦听器）

[关于computed实现](https://juejin.im/post/6844903606676799501#heading-10)

> `computed` 只有在相关的属性（此处是allName）发生变化才计算\(官方文档写的是会被缓存\) 使用`methods`计算属性，则是每一个数据发生变化就会执行,性能消耗大。

```javascript
    computed: {
        // 此处的allName、reverseMessage也属于data对象内的值，即同名会产生冲突
        allName: {
            get() {
                return this.firstName + this.secondName;
            },
            set:function(value) {
                console.log(this);
                let arr = value.split(' ')
                this.firstName = arr[0];
                this.secondName = arr[1];
            }
        },
        //计算属性的getter函数
        reverseMessage: function () {
            // console.log('computed run');
            return this.message.split(' ').reverse().join(' ')
        }
    },
```

> **注** `watch` 内禁止使用箭头函数，类似于回调函数（对于组装数据代码量明显大于 `computed`）

```javascript
    watch: {// 侦听函数，有缓存
                firstName: function () {
                    console.log('watch');
                    this.fullName = this.firstName + this.secondName;
                },
                secondName: function () {
                    console.log('watch');
                    this.fullName = this.firstName + this.secondName;
                }
            },
```

### 关于属性访问

> Vue给原生包裹了一层 `definePerproty()`
>
> 即 `vm.$data.msg`等价于 `vm.msg` **（此处的vm指的是Vue实例）**

### 动态绑定类名

> 静态类型：`nav-item` 动态类名：`nav-active`

```javascript
//1
class="nav-item"
:class="{'nav-active': index === curIndex}"

//2
:class="['nav-item',index === curIndex ? 'nav-active' : '']"

//3
:class="['nav-item',{'nav-active':index === curIndex}]"
```

### 封装动态绑定类名（自定义指令）

> 原理通过vue原有的api进行`原生的js`操作，实现复杂操作，达到封装指令的效果

**指令**

```javascript
export default {
    bind(el, binding) { // el 是root节点 ，binding 是 bind的节点
        // console.log(el, binding);
        const options = binding.value;
        const { className, activeClass, curIndex } = options;
        const children = el.getElementsByClassName(className);
        children[curIndex].className += ` ${activeClass}`;
    },
    update(el, binding) {
        const options = binding.value;
        const oldOptions = binding.oldValue;
        // console.log(el, binding);
        const { className, activeClass, curIndex } = options;
        const children = el.getElementsByClassName(className);
        const { curIndex: oldCurIndex } = oldOptions;
        children[curIndex].className += ` ${activeClass}`;
        children[oldCurIndex].className = className;
    },
}
```

**指令使用**

```javascript
<template>
  <div class="nav-bar" v-nav-active="{
      className:'nav-item',
      activeClass:'nav-active',
      curIndex
  }">
    <div
      v-for="(item, index) in items"
      :key="index"
      @click="changeIndex(index)"
      class="nav-item"
    >{{item}}</div>
  </div>
</template>

<script>
import NavActive from "../directive/navActive";
export default {
  directives: {
      NavActive
  },
  data() {
    return {
      items: ["one", "two", "three"],
      curIndex: 0,
    };
  },
  methods: {
    changeIndex: function (index) {
      this.curIndex = index;
    },
  },
};
</script>

<style>
.nav-bar {
  width: 400px;
  height: 50px;
  margin: 0 auto;
  border: solid black 1px;
}
.nav-item {
  float: left;
  width: 33.3%;
  height: 100%;
  line-height: 50px;
  text-align: center;
}

.nav-active {
  background-color: black;
  color: white;
}
</style>
```

### Render函数

> render 函数最终还是被编译成template，但是比起template灵活性高**（即使用JS写页面）**
>
> Vue 选项中的 `render` 函数若存在，则 Vue 构造函数不会从 `template` 选项或通过 `el` 选项指定的挂载元素中提取出的 HTML 模板编译渲染函数。

```javascript
<script>
export default {
  props: {
    tag: String,
  },
  data() {
    return {
      people: ["zoulam", "luluxi", "lala"],
    };
  },

  render(createElement) {
    // arg1 组件名/标签名
    // arg2 标签的详细设置
    //  agr3：子元素
    // return createElement('h1',{},'hello vue')
    // 动态渲染
    return createElement(
      this.tag,
      {},
      this.people.map((name) =>
        createElement("li", { attrs: { class: "test" }, on: {
            click:()=>{
                console.log('li click!');
            }
        } }, `${name}`)
      )
    );
  },
};
</script>

<style>
</style>
```

```javascript
<template>
  <div id="app">
    <h1>render()</h1>
    <render  tag="ul"/>
  </div>
</template>

<script>
import render from './components/render'
export default {
  components:{
    render,
  },
};
</script>

<style>
#app{
  text-align: center;
}
</style>
```

### 为什么组件的data必须是函数

> 利用闭包的原理，这样组件之间的数据就是独立的，互补干扰

### 父子组件之间的通信方式

#### `props + $emit()`

#### 回调函数

#### `$parent + $children`

#### `provide + inject`

#### `$attrs +$listeners`

#### `$refs`

