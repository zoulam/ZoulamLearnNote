# vue的实践

## 单体组件

1、创建实例 `new Vue(optional)`，创建一个vue对象（通常被称为 `vm`即vue-model），上面挂载了大量的内容，包括data等，我们可以通过 `vm.xx`直接获取数据是因为vue帮我们做了一层拦截。

2、传输选项

​		el

​		data

​		methods

3、通过大量的 `v-`指令来实现功能，而不是像之前那样操作 `DOM`

```javascript
<div id="root">
    {{message}}
</div>
<script>
    const vm = new Vue( {
        el:"#root",
        data:{
            message:"hello vue"
        }
    });
</script>
```

## 组件化

`Vue.component(组件名, {})`

> 组件命名强制要求：
>
> 短横线命名`kebab-case` 如：`todo-item`
>
> 大驼峰`PascalCase` 如：`TodoItem`
>
> 使用时则强制为： `<todo-item><todo-item>`



>  参数二：
>
> ​	props：数组 | 对象
>
> 同样也有跟组件一样的属性
>
> ​	data （他是一个函数）
>
> ​	methods
>
> ​	template

```javascript
// todos 是从父组件的data中取得
// 使用incomingValue表明是通过这个变量传入的
// 通过v-bind 传入
<todo-item v-for="item in todos" v-bind:incomingValue="item" v-bind:key="item.id"></todo-item>

Vue.component('todo-item', {
    props: ['incomingValue'],
    template: '<li>{{ incomingValue.text }}</li>'
})
```

## render函数和template

>  `render`函数默认传入`createElement`函数作为参数

```
render(createElement){

}
```



## vm实例上的方法和属性

命名都是以 `$`开头的