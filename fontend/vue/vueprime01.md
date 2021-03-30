# 入门

开发版本有警告，生产版本无警告（文件体积小）

# 生态

官方：

- vue 
- vuex （状态管理，方便组件间通信）
- vue-router （路由）
- vue-cli （基于webpack的脚手架）
- vite （新一代的高性能脚手架，vite核心功能是使用ESmodule实现热更新，rollup进行打包）
- devtool （浏览器插件）
- vue-server-renderer （服务端渲染）
- vue-loader （编译template等vue语法）
- vue-style-loader（样式loader，适用于webpack）
- vetur（代码补全）
- wexx (和阿里一起弄的写小程序)

非官方：

- mpvue（使用vue语法写小程序） 
- nuxt（native app）
- vant（组件库，有web、小程序等版本）
- Element（组件库）
- AntDesign（组件库，React版本维护较好）

# 浅谈API

Vue有这样几类API：

1、Vue.config上的配置项

2、Vue上的全局API，Vue.component等

3、作为参数的optionsAPI，上面有生命周期钩子函数、选项和数据

4、实例化对象的属性，为了方便获取optionsAPI，周期函数，父子节点，等等，`vm.$data`  `vm.$children`  `vm.$mount`

5、指令，写入到template或者html内的指令，以 `v-`开头

6、指令之外的attribute：ref、is、key、slot、slot-scope、scope直接写在html内的内容

7、内置组件，`<componet/>`、`<transition/>`、`<transition-group/>`、 `<keep-alive/>`、 `<slot/>`

8、生态范围内的组件 `<route-link to=""><route-link/>`  `<route-view></route-view>`

# 推荐学习资料

2.0

[Vue.js 技术揭秘](https://ustbhuangyi.github.io/vue-analysis/)

[Vue2.1.7源码学习](http://hcysun.me/2017/03/03/Vue%E6%BA%90%E7%A0%81%E5%AD%A6%E4%B9%A0/)

3.0

[Vue.js 3.0 核心源码解析](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=326#/content) **付费**

[深入理解 Vue3 Reactivity API](https://zhuanlan.zhihu.com/p/146097763)

[渲染器](http://hcysun.me/vue-design/zh/essence-of-comp.html)

[Vue3.0学习教程与实战案例](https://vue3.chengpeiquan.com/)

有趣的文章

[他写出了 Vue，却做不对这十道 Vue 笔试题](https://zhuanlan.zhihu.com/p/231510566)

编译环境

[vuese-explorer](https://vuese.github.io/vuese-explorer/)

# 需要掌握的内容

响应式原理

双向绑定：响应式 + onchange

组件化开发

v-if 和 v-show

v-if 和 v-for为什么不能连用

组件中的data为什么是函数的返回值

SPA和传统的多页面对比

单向数据流原则

computed、watch、methods区别和使用场景

生命周期的理解（哪里之后能获取到dom节点，哪里之后能获取data）

父子组件通信的方式

keep-alive

SSR的优缺点，解决了什么问题

vue-router的多种模式，使用场景

router和route的区别

vite的tree-shaking原理

组件的编写方式 （Vue.component，JS对象组件，函数组件，类组件，Vue.extend实例化（可以复用逻辑）+$mount（需要在onMounted上使用，不然会出现找不到节点的问题），mixin按需加载组件）

​		Vue.use(Plugin, {options})，根据options传入的参数进行匹配，再使用 Vue.component注册。

```JavaScript
// 使用
Vue.use(MyUI, {
	components: [
		'button',
		'input'
	]
})
// 创建
MyUI = {}
const COMPONENTS = [
   BUTTON
]
// Vue上下文，避免每个插件都要单独引入Vue源码，但是出现版本不一致的话也是需要做出引入处理的
MyUI.install = function (Vue,options) {
	// Vue.compoent
	// Vue.directive
	// Vue.mixin
	Vue.prototype.$http = function(){}
	if(options && options.components) {
		const components = options.components
        components.forEach((comp) => {
            COMPONENTS.forEach((component) => {
                if(comp == component.name) {
					Vue.component(component.name, component)        
                }
			})
        })
	} else {
        // 默认全部注册
    }
}


<button @click="btnClick($event)"></button>
name:'my-button',
methods:{
    btnClick: function(e) {
        this.$emit('click', e)
    }
}

<my-button @click="output"></my-button>

methods:{
    output: function (e){
        console.log(e) // 拿到上面的$event参数
    }
}


// 配置
import {MyButton} from 'MyUI'
Vue.prototype.$ELEMENT = { size: 'small', zIndex: 3000 };
// 只引入按钮
Vue.use(MyButton);

// 实现
MyButton = {}
MyButtonButton.install = (Vue) => { Vue.component(MyButton.name, MyButton) }
export {MyButton}
```

teleport使用场景

mixin的使用场景：

​	vuerouter和第三方组件库上都使用到了

如何开发第三方组件库

​	第三方组件库，需要使用vue.use方法，传入vue上下文和调用内部install方法。

​	即最小结构： 类和类上包含install方法

vuex

 state,  // 存放数据

 mutations,// 存放方法，命名是setXxx（Xxx是数据名）， setValue(oldVal, newVal)

 actions, // 异步更新上下文数据，再触发mutations更新

 modules，// 以文件夹的拆分模块 setValue(ctx,payload) ctx.commit(mutations上的方法,新的数据)

 getters, // 处理重复使用的数据，如，需要重复拼接的名字等

vue-router

`<router-link to="path" />`

`<router-view />`

vue3的源码上做出的优化：

​	1、项目使用monorepo开发，各个库独立发布

​	2、使用typescript重写源码，代码提示更好

​	3、性能优化：

​			3.1、压缩代码体积（移除冷门无用的api），tree-shaking（uglify-js、terser 等工具实现）

​			3.2、数据劫持优化（Object.defineproperty => Proxy），过去无法检测对象属性的设置和删除，需要使用`$set` 和 `$delete` 实例方法。同时data中的数据嵌套过深（数据扁平化优化）。过去无脑递归，现在需要数据才递归。

​	4、编译优化

​		只对动态模板（绑定了动态数据的模板部分）进行DOMdiff

vue3对开发的优化：

1、optionsapi是写法是数据和数据操作是分离的，compostionAPI则是数据和对应操作写在一起，代码更加内聚。

2、优化逻辑复用，旧版使用mixin（存在命名冲突和数据来源不清晰的问题），新版使用自定义hook函数实现逻辑复用。

# 使用方式

```JavaScript
html -> ((template | html) + vuedirectives)
css -> css | scss | less | vm.$data   <style lang="scss"  scoped>
javascript -> javasript | ts

// 结构
// 访问数据
vm.StyleObj // data上面的数据使用的 defineProperty包裹可以直接访问
vm.$data.StyleObj

new Vue(el,data,components,methods,watch,computed,) || new Vue().$mount("#app")
{name,props,data,template,……}


const vm = new Vue({
    el: "#app",
    data: {
        StyleObj: {
            color: "red"
        },
        arr: ["lala", "nana", "bobo"],
        message: "hello world",
        isShow: true,
        htmlMSG:`<h1>v-html</h1>`,
        value:""
    },
    methods: {
        handleBtnClick: function() {
            console.log("hello world")
		},
        btnChange: function() {
            this.isShow = !this.isShow
        }
    }
})

// 使用data的方式：
<div id="app">
// 原生事件
<div @click="handleClick"></div>    
// 自定义事件
<child-btn @click="handleClick"></child-btn>  
// 原生事件
<child-btn @click.native="handleClick"></child-btn>  
// 动态传值和静态传值传入字符串
<div content="value"></div>
<div content="123"></div> <!--字符串-->
<div :content="'value'"></div>
<div :content="123"></div> <!--数字-->
<div v-bind:style="StyleObj" v-on:click="">click</div>
<div v-text="message"></div>
// 语法糖
<div :style="StyleObj" @click="">click</div> // v-bind的语法糖，用于动态绑定data内的数据
<div>{{message}}</div>
// 列表渲染
// 添加上key提高虚拟DOM的性能
<div v-for="(item, index) of arr" :key="">{{index}}:{{item}}</div>
<div v-for="item of arr">{{item}}</div>
// 条件渲染，从页面移除（创建性能高，多次创建销毁性能低）
<div v-if="type== 'A'">A</div>
// <span></span> // 导致报错,v-else-if 无法找到对应的v-if
<div v-else-if="type== 'B'">B</div>
<div v-else-if="type== 'C'">C</div>
<div v-else>D</div>
// 条件渲染，使用display:none消失（创建性能低，多次创建销毁性能高）
<div v-show="isShow" @isShow="btnChange">isShow</div>
// 插入html文本，小心XSS攻击
<div v-html="htmlMSG"></div>
// 仅限于<input><select><textarea>这几个标签以及                  vue组件
// 修饰符的语法就是在指令后面添加冒号和指令符
<input v-model:trim="value"/> 
<div>{{value}}</div>
</div>

<template>
    <div v-slot=""></div>
	// 被识别为普通节点，不会被vue编译
	<div v-pre>{{ this will not be compiled }}</div>
	// 只渲染一次，跳过后续的编译检查
	<div v-once></div>
</template>


// 极简结构
cosnt vm = new Vue({
    el: "#app",
    components: {
        key: componentName
    }
    data: {},
    methods: {}, //
    watch: {}, // 
    computed: {} // 
})

// 三剑客
const vm = new Vue({
    methods: {
        
    },
    // 比methods性能高，比watch（一个一个属性监听，computed是计算多个属性）简洁
    // 对数据操作并返回，getter函数内自动执行
    // 只对被监听的数据发生变化才执行,methods只要页面内容变化就执行
    // computed 属性是函数时默认是getter
    // 属性对象的时候可以是get、set函数
    computed: {
        name: function (){
            
        },
        full: {
        	get: (){
            
        	},
        	set: (value){
        		// 可以二次处理value
        	}
		}
    },
    // 侦听属性,有缓存，key指的是data上的属性，发生变化就执行后面的函数
    watch: {
        key: fuction() {
        
		}
    }
})

vue.component({
    // 包含根组件的之外还包含下面的内容
    template:"<h1>hello vue</h1>"
    // 属性校验通常是组件库需要考虑的事情
    props: [string|Number]|{} // 子组件接收外部值
    // 作为对象可以使用校验      
	//    type（类型,单个类型是字符串，多个类型是数组） 
	//    default（默认值） 
	//    required（是否必填） 
	//    validator（函数返回值作为校验）
	data: function (){
        return { // 利用闭包，保证每个子组件的数据独立
            
        }
	}
})
```

# API

api分类

- vue配置api
- 全局api
- 指令
- 实例化vue的可选|数据

# 组件

>  好处：可以复用，方便调试，便于协同开发

## 规范

组件声明：

~~简单命名 `child` **\(开发中禁止使用\)**~~

短横线命名`kebab-case` 如：`list-item`

大驼峰`PascalCase` 如：`TodoItem`

组件使用：

强制使用 `<todo-item><todo-item>`

## 全局注册

>  即便不适用，也一定会被打包进代码内。
>
> ​	同时全局组件之间可以相互嵌套。

```JavaScript
// 传值
<todo-item :content="item" v-for="item of arr"></todo-item>
Vue.component("TodoItem",{
    props: ['content']
	template:'<li>{{content}}</li>',
})
```

## 局部注册

>  以JavaScript对象的形式创建

```JavaScript
<todo-item :content="item" v-for="item of arr"></todo-item>

let todo-item-a = {
	props: ['content']
	template:'<li>{{content}}</li>',
}

const vm = new Vue({
    components: {
        'todo-item' : todo-item-a // 使用的时候用key，相当于重命名
    }
})

<todo-item></todo-item>
```

## Vue.extend + $.mount

```JavaScript
<div id="mount-point"></div>
var Profile = Vue.extend({
  template: '<p>{{firstName}} {{lastName}} aka {{alias}}</p>',
  data: function () {
    return {
      firstName: 'Walter',
      lastName: 'White',
      alias: 'Heisenberg'
    }
  }
})
// 创建 Profile 实例，并挂载到一个元素上。
new Profile().$mount('#mount-point')
```

## $emit()

>  **组件**自定义事件和传参问题

<img src="https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20210318222710150.png" alt="$emit" style="zoom:67%;" />

```javascript
<div id="app">
    <todo-item @delete="change" :content="item" :index="index" v-for="(item, index) of arr">
    </todo-item>
</div>

Vue.component("TodoItem", {
    props: ['content', 'index'],
    template: '<li @click="handleClick">{{content}}</li>',
    methods: {
        handleClick: function () {
            this.$emit('delete', this.index)
        }
    }
})

new Vue({
    el: "#app",
    methods: {
        change: function (index) {
            console.log(index)
            console.log("oh my god")
        }
    },
    data: {
        arr: ['name', 'lala', 'change']
    }
}
```

## render和template和JSX

>  render 会让Vue放弃检查template

```JavaScript
render: function (createElement) {
  return createElement(
      {String(普通html标签) | Object（组件） | Function（createElement函数）},
      {Object}, // innerText
      {String | Array}// 子组件
  )
}
```

```JavaScript
<div id="app">
// show是true也不会被渲染，类似于React的 <></> 文档碎片
	<template v-if="show">
		<div>1</div>
		<div>1</div>
		<div>1</div>
	</template>
</div>
```

# Vue.use

使用插件，可以链式调用

```JavaScript
Vue.use(插件1).use(组件库)
```

# 生命周期

`vm.$destory()`

从父节点上移除`$children`

就是将自身的依赖追踪和事件监听移除

移除响应式数据

# 绑定类名

```javascript
// 短横线类名需要加单引号，不然会解析错误
<div :class="{active: isActive, 'btn-lg': isBig}"></div>
// 从data上获取isActive，如果是true的时候添加上类名active
const vm = new Vue({
    data:{
		isActive: true
    }
})

// 更加常用的方法
<div :class="classObj"></div>

const vm = new Vue({
    data:{
        classObj: {
            active: true,
            bnt-lg: true
        }
    }
})

// 不怎么好用的数组语法
<div :class="[activeClass, btnClass]"></div>
const vm = new Vue({
    data:{
		activeClass: 'active',
        btnClass: 'bnt-lg'
    }
})
```

# 内联样式

```JavaScript
<button :style="{color:'red', fontSize:'18px'}">style</button> // 驼峰命名
<button :style="styleObj">style</button> // styleObj写在data内
<button :style="[color, other]">style</button> // color other写在data内
```

# 被缓存的问题

>  默认现实email，在email中输入，然后 vm.isShow = true后发现输入内容被缓存，**v-for不添加key**也会让缓存的内容错乱。

```JavaScript
<div id="app">
    <div>
        <div v-if="isShow">
            username:<input type="text" />
        </div>
        <div v-else>
            email:<input type="text" />
        </div>
    </div>
</div>
<script>
    const vm = new Vue({
        el: " #app",
        data: {
            isShow: false
        }
    });
</script>
```

## 修改data

Vue.set(vm.target, propertyName/index, value)，用于修改data中的数据

vm.$set(vm.target, propertyName/index, value)

vm.target.splice(),使用vue重写的方法

vm.target = newTarget重新赋值

# 节点嵌套异常

>  HTML5规范规定tbody内必须是tr，所以组件会被渲染到外部

```JavaScript
<div id="app">
    <table>
        <tbody>
            <tr>
                <td>1</td>
                <td>2</td>
                <td>3</td>
            </tr>
            <tr>
                <td>1</td>
                <td>2</td>
                <td>3</td>
            </tr>
        </tbody>
    </table>
    <!-- 嵌套混乱row-component被插入到table外部 -->
    <table>
        <tbody>
            <row-component />
            <row-component />
            <row-component />
        </tbody>
    </table>
    <!-- 正确 -->
    <table>
        <tbody>
            <tr is="row-component"></tr>
            <tr is="row-component"></tr>
            <tr is="row-component"></tr>
        </tbody>
    </table>
</div>
<script>
    Vue.component('row-component', {
        template: "<tr><td>error1</td><td>error2</td><td>error3</td></tr>"
    })
</script>
```

## ref

>  ref用于获取dom节点

```JavaScript
<div id="app">
    <div ref="hello">hello</div>
    <div ref="hi">hi</div>
</div>
<script>
    const vm = new Vue({
        el: " #app",
        data: {
            isShow: false
        }
    });
    console.log(vm.$refs['hello']) // <div>hello</div>
    console.log(vm.$refs['hi']) //  <div>hi</div>"
</script>
```

# 组件间传值

## **父 -> 子**

> 动态绑定，子的props
>
> **注：**禁止直接修改props的值，这样其他组件的值也会被影响。
>
> **正确：**使用data接受props，然后再修改自己内部的data，而不是修改外部传入的props。

```JavaScript
<!--单向数据流-->
<div id="app">
    <child-component :content="value"></child-component>
    <div>{{value}}</div>
</div>
<script>
    Vue.component("child-component", {
        data: function () {
            return {
                number: this.content
            }
        },
        props: ['content'],
        template: '<div @click="change">{{number}}</div>',
        methods: {
            change: function () {
                this.number++
                // this.content++ // 不会被执行，但是vue会报错
            }
        },
    })
    new Vue({
        el: "#app",
        data: {
            value: 1
        }
    })
</script>
```

### 单向数据流



```javascript
<div id="app">
    <child-component :content="value"></child-component>
</div>
<script>
    Vue.component("child-component", {
        props: ['content'],
        template: '<div>{{content}}</div>'
    })
    new Vue({
        el: "#app",
        data: {
            value: "fatherValue"
        }
    })
</script>
```

### 非props属性被继承

>  非props的属性会被继承到组件**根元素**上，可以设置 inheritAttrs

```JavaScript
<child-component :content="value" other="123"></child-component>
// 被编译成
<div other="123">fatherValue<div>child</div></div>
```

```JavaScript
<div id="app">
    <child-component :content="value" other="123"></child-component>
</div>
<script>
    Vue.component("child-component", {
        // inheritAttrs: false, // 阻止继承行为，除了style和class属性
        props: ['content'],
        template: '<div>{{content}}<div>child</div></div>'
    })
    new Vue({
        el: "#app",
        data: {
            value: "fatherValue"
        }
    })
</script>
```

## **子->父**

this.$emit("xx", 此处传参)

```javascript
<div id="app">
    <child-component @change="test"></child-component>
</div>
<script>
    Vue.component("child-component", {
        template: '<button @click="hanleClick">test</button>',
        methods: {
            hanleClick: function () {
                return this.$emit("change", "childrenValue1", "childrenValue1")
            }
        }
    })

    new Vue({
        el: "#app",
        methods: {
            test: function (value1, value2) {
                console.log(value1, value2)
            }
        }
    })
</script>
```

## **兄弟**

非同父子，传递到公共祖先，再向下传递（x）

1、Vuex

2、总线 + `$on`，创建bus挂载到Vue的原型链上（可以用this取得），同时给他挂载Vue实例（获取vue的 `$on`方法）

```javascript
<div id="app">
    <child-component content="hello"></child-component>
    <child-component content="bye"></child-component>
    <child-component content="yes"></child-component>
</div>
<script>
    Vue.prototype.bus = new Vue()

    Vue.component("child-component", {
        data: function () {
            return {
                text: this.content
            }
        },
        props: ['content'],
        template: '<div @click="handleChange">{{text}}</div>',
        methods: {
            handleChange: function () {
                this.bus.$emit('change', this.content)
            }
        },
        mounted: function () {
            const _this = this
            this.bus.$on('change', function (msg) {
                _this.text = msg
                // console.log(msg) // 上面有三个child-component点击会被打印三次
            })
        }
    })
    new Vue({
        el: "#app",
    })
</script>
```

# slot

> 语法改为：v-slot 旧版的slot和slot-scope被废弃（建议不使用）
>
> ​	old： `<div slot="slotName"> </div>`
>
> ​	new: `<template  v-slot:slotName></template>`
>
> 插槽，理解为用户自定义部分，组件为可复用部分

```JavaScript
<div id="app">
    <child>
        <p>hello world</p>
    </child>
</div>
<script>
    const chidlren = {

        template: `<div><slot></slot></div>`
    }
    const vm = new Vue({
        el: "#app",
        components: {
            child: chidlren
        }
    })
</script> `
```

# 作用域问题（slot**子->父**）

```javascript
<div id="app">
    <child content="childrenValue" v-slot="slotProps">
        {{user.name}}
        {{content}} // 插槽无法取得注入到子组件props上的值，必须从子组件的template获取
        {{slotProps.value}} // 子组件挂载后取用
    </child>
</div>
<script>
    const chidlren = {
        props: ['content'],
        template: `<div><slot :value="content"></slot></div>`
    }
    const vm = new Vue({
        el: "#app",
        data: {
            user: {
                name: "fatherValue"
            }
        },
        components: {
            child: chidlren
        }
    })
</script>
```

# 具名插槽

```JavaScript
<div id="app">
    <child>
        <template v-slot:header>header</template>
        <template>middle</template>
        <template v-slot:footer>footer</template>
    </child>
</div>
<script>
    const chidlren = {
        template: `<div>
            <slot name="header"></slot>
            <slot></slot>
            <slot name="footer"></slot>
            </div>`
    }
    const vm = new Vue({
        el: "#app",
        components: {
            child: chidlren
        }
    })
</script>
```

#### `<transition>`

用于设置**单个**元素的的过渡效果，实现方式（添加类名）

#### `<transition-group>`

设置**多个**元素的过渡效果，原理是给每一个标签单独设置一个`<transition></transition>`

### js写动画

> 配合**Vue**的钩子函数、 `setTimeout` API以及css的`transition`属性，实现使用纯JS写动画效果。

### 组件设计

> 全局组件

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

