# 原型\(proto\)、原型链（proto chain）

> 原型链的终点是`Object.prototype`







```
|this设置的值
|proto
	|constructor	
		|Object.[value]添加的值
		|prototype 里面包含着Object.prototype={}
		|__proto__
	|声明在构造函数前
	|Object.prototype.[value]添加的值
	|super_class:继承的父亲构造函数
	|proto
		|constructor:父亲构造函数
		|proto
			|constructor：Object()
			|操作对象的方法
			|……
			|……
			|……
```



```

```

