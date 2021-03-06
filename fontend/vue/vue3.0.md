# 脚手架使用

1、脚手架创建项目

```bash
vue create <projectName>
```

2、选项

```bash
? Please pick a preset: Manually select features # 选择自定义配置

? Check the features needed for your project:
 (*) Choose Vue version # 勾选才能获取vue3 不然默认是 vue2
 (*) Babel
 (*) TypeScript
 ( ) Progressive Web App (PWA) Support
 (*) Router
 (*) Vuex
 (*) CSS Pre-processors
>( ) Linter / Formatter # 关闭
 ( ) Unit Testing
 ( ) E2E Testing
 
 
 
 ? Choose a version of Vue.js that you want to start the project with
  2.x
> 3.x (Preview)


? Use class-style component syntax? (y/N) n # 不需要类组件

? Use Babel alongside TypeScript (required for modern mode, auto-detected polyfills, transpiling JSX)? (Y/n) n # 使用template语法而不是JSX

? Use history mode for router? (Requires proper server setup for index fallback in production) (Y/n) y # 选择history路由模式，即不带#号的路由

? Pick a CSS pre-processor (PostCSS, Autoprefixer and CSS Modules are supported by default): (Use arrow keys)
> Sass/SCSS (with dart-sass) # dart-sass 速度比较快，而且更新比node-sass快
  Sass/SCSS (with node-sass)
  Less
  Stylus
  
? Where do you prefer placing config for Babel, ESLint, etc.? (Use arrow keys)
> In dedicated config files # 生成独立的配置文件
  In package.json
  
? Save this as a preset for future projects? (y/N) n # 不保存选项给后续使用
```

# 脚手架搭建目录分析

```bash
asset # 静态资源，图片等
components # 可复用组件
router # 路由配置，写webpack的名字是使用jsonp加载页面
store # vuex数据仓库
views # 视图，也就是一个个的子页面
App.vue # 主页面
main.ts # 也就是插件挂载app节点插入
shims-vue.d.ts # declare类型声明文件 shims中文意思是垫片的意思，只要导入了这里的声明就会有TS语法提示

# 自己新增
api 
hooks
typings/home.ts # TS类型声明
typings/…….ts # TS类型声明
store/modules 
views/home/index.vue
views/mine/index.vue
views/profile/index.vue
```

删除不需要的文件

```bash
# 删除模板的
# components/HelloWorld.vue 组件文件
# 删除views/About.vue 文件
# 删除views/Home.vue下的剔除HelloWorld.vue引入


# app.vue删除无用信息
<template>
  <div id="nav">
  </div>
</template>

<style lang="scss">

</style>
```

编写组件的提示功能

```JavaScript
<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({ // defineComponent是vue内部声明的类

});
</script>
```

修改路由

# 引入vant

[vant](https://vant-contrib.gitee.io/vant/#/zh-CN/) 

这玩意有支持微信小程序的组件一个库 [vant3](https://vant-contrib.gitee.io/vant/v3/#/zh-CN)支持vue3

```bash
npm i vant@next -S
```

导入方式有全量导入和按需引入

```JavaScript
import Vant from 'vant'
import 'vant/lib/index.css' # 引入样式库
createApp(App).use(Vant)
```

### 接口地址

```javascript
www.fullstackjavascript.cn:3000/slider/list
http://fullstackjavascript.cn:3000/lesson/list?category=1
```



## 优点介绍

最长子序列优化，将算法从 n^2优化为nlogn

|                | vue2                                                         | vue3                                                         |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 源码的类型支持 | flow                                                         | TS(更好的代码提示)                                           |
| 源码体积优化   | 移除部分api（filter）                                        | 函数API，方便tree-shaking                                    |
| 数据劫持优化   | `defineProperty`        深层递归，过去优化就是flatten（扁平），即不要在深层级中使用双向绑定。 | `Proxy` and Reflect                                          |
| 编译优化       |                                                              | 静态标记不需要变成dom节点，重写diff算法                      |
| api变化        | `optionsAPI`会出现对象过度膨胀的问题，`mixin` 提取公共逻辑，**会有命名冲突的问题和数据来源不清晰的问题** | `composition` `api`                                          |
| 自定义渲染器   |                                                              | 改写Vue底层的渲染功能（小程序，canvas，webgl定制渲染dom元素） |
| 新增组件       |                                                              | `Fragment` `Teleport` `Suspense`                             |

过去使用类的装饰器语法，所以API以属性写入，新版使用TS，就改成函数API

使用组合原先的多个方法（解决高阶组件逻辑复用问题）

## 关于使用

### 目录分析

```text
compiler：编译相关 core是核心（平台无关） dom（浏览器专属）
runtime：代码运行时相关 core是核心（平台无关） dom（浏览器专属） test（提供用户测试）
reactivity：响应式实现相关
server-renderer：服务端渲染
shared：共享代码
template-explorer：模板解析
vue：代码总的打包（主要包括compiler和runtime）
```

先 `git clone https://github.com/vuejs/vue-next.git`

然后 `npm install` 安装依赖

使用`npm run dev` 将vue从TS编译成JS,这样可以查看源码

引入编译后的Vue `packages/vue/__test__` 下就是Vue的测试文件，可以参照他的测试方式依葫芦画瓢

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <div id="container"></div>
    <script src="./vue.global.js"></script>
    <script>

        // 封装公共函数 函数以use开头
        function usePosition() {
            let position = Vue.reactive({ x: 0, y: 0 });
            function update(e) {
                position.x = e.pageX;
                position.y = e.pageY;
            }
            // 生命周期只属于使用 usePosition函数的对象
            Vue.onMounted(() => {
                window.addEventListener('mousemove', update)
            })

            Vue.onUnmounted(() => {
                window.removeEventListener('mousemove', update)
            })
            // { x: { value: 0 } }  toRefs包装基本数据类型为对象
            return Vue.toRefs(position)
        }

        const App = {
            setup() {// 类似created，只会执行一次性能更好
                let value = Vue.reactive({ name: 'zoulam' })
                let { x, y } = usePosition();
                function change() {
                    value.name = 'luluxi';
                }
                return {
                    value,
                    change,
                    x, y,
                }
            },
            template: `<div @click="change">{{value.name}} x:{{x}} y:{{y}}</div>`
        }

        Vue.createApp(App).mount(container)
    </script>
</body>

</html>
```

## 新的响应式

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <div id="container"></div>
    <script src="./vue.global.js"></script>
    <script>
        let proxy = Vue.reactive({ name: 'zoulam' });
        // 副作用
        Vue.watchEffect(()=>{
            console.log(proxy.name);
        })
        proxy.name = 'luluxi';
        proxy.name = 'zhangsan';
    </script>
</body>

</html>
```

`{user:{name:'zoualm'}}`

1、vue2.0中的双向绑定使用`Object.defineProperty(obj, key, {options})`实现，对深层对象的变化需要用**默认**递归实现绑定，内存消耗大。

2、若对象是数组，无法实现数据方法导致的变化，Vue内部使用原型重写了数组方法，然而对数组下标和 `length`的操作也是无法进行双向绑定的

3、对对象中不存在的属性（即后续添加的）无法进行数据劫持，也得使用递归实现

