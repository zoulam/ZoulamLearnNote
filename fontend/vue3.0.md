# \[vue\]3.0

## 优点介绍

过去使用类的装饰器语法，所以API以属性写入，新版使用TS，就改成函数API

使用composition api组合原先的多个方法（解决高阶组件逻辑复用问题）

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

```markup
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

