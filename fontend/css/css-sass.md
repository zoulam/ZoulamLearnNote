# \[css\]sass

> 基于ruby语法开发是css处理器

## react环境

```bash
cnpm install node-sass -D # cra就不用配置，自己搭建需要配置scssloader规则
```

## 文件组织

```bash
// 全局样式
# 直接引入到 App.js的样式 这一文件是引入其他scss文件的入口，引入是省略下划线
styles/index.scss 
# 也是自定义mixin的一种不过单独划分了文件
styles/_animation.scss
# 自定义mixin
styles/_mixin.scss
# scss中去除浏览器默认样式并做出美化，实现各个浏览器的表现是一致如：设置border-box为content-box
styles/_reboot.scss 
# 存放大量的变量
styles/_variables.scss

// 局部样式
Components/Component1/_style.scss
Components/Component2/_style.scss
```

## scss规则

1、变量声明

```css
$<variabelName> : <cssVariable> !default;
$warning:     null;
$warning:     red !default;
#main{
    color: $warning;
}

#main{
    color: red;
}
```

插槽值

```text
#{}
类似于JavaScript的 ${}可以在文本中间插入变量
```

数据结构

```css
map创建
$primary:     red !default;
$theme-colors:
(
  "primary":    $primary,
)

map使用，当元素的类名是： icon-primary 就会变成红色
@each $key, $val in $theme-colors {
  .icon-#{$key} {
    color: $val;
  }
}
```

强制全局

```text
scss存在块级作用域，但可以使用!global将变量提升到全局
```

2、计算

```text
// 会进行单位换算
+, -, *, /, %
```

3、模块化

```text
@import "path"
```

自定义mixin

```text
@mixin
```

