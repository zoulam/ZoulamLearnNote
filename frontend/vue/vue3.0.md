# Vue3.0

`{user:{name:'zoualm'}}`

1、vue2.0中的双向绑定使用`Object.defineProperty(obj, key, {options})`实现，对深层对象的变化需要用**默认**递归实现绑定，内存消耗大。

​	`proxy`则按情况使用递归，因为`Reflect`对象中有返回值，可以提供判断。

2、若对象是数组，会出现数组方法支持不完全，即数组方法是vue重写的，并且缺少`length`属性

3、对对象中不存在的属性无法进行数据劫持



