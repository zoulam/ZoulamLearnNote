# \[react\]-dev-env

## 流程

1、安装webpack

```text
npm init -y
npm install webpack -D
npm install webpack-cli -D
```

2、写配置

```text
webpack.common.js // 通用配置文件
```

3、安装babel

```bash
cnpm i  @babel/core @babel/preset-env @babel/preset-react babel-loader @babel/transform-runtime-D

@babel/core
@babel/preset-env # es
@babel/preset-react # jsx
babel-loader  
@babel/transform-runtime # generator 和promise
```

3.1写配置

```text
babel.config.js
```

4、安装react

```text
cnpm install react react-dom -S
```

5、安装 `html-webpack-plugin`

```text
cnpm install html-webpack-plugin -D
```

6、本地服务器

```text
cnpm install webpack-dev-server -D
```

7、typescript

```text
配置文件生成：
npx ts --init 【建议自己修改配置文件】
cnpm install typescript @types/react @types/react-dom -D
```

8、css-loader

```text
cnpm install style-loader css-loader -D
```

9、sass-loader

```text
cnpm install sass-loader node-sass -D
```

10、postcss

创建配置文件

```bash
postcss.config.js
cnpm install postcss-loader autoprefixer -D
cnpm install postcss-preset-env -D # 不用写配置
```

11、css module优化

```text
cnpm install url-loader file-loader -D
```

12、eslint

```text
cnpm install eslint eslint-plugin-react eslint-plugin-react-hooks @typescript-eslint/eslint-plugin @typescript-eslint/parser -D
```

13、antd

```text
cnpm install antd -S
cnpm install babel-plugin-import less less-loader -D
```

13.1优化icon

```text
按需引入
src 下新建 icons.ts
export { default as DownOutline } from '@ant-design/icons/lib/outline/DownOutline'
```

14、添加更多babel

```text
cnpm install @babel/preset-typescript @babel/plugin-proposal-class-properties @babel/plugin-proposal-decorators @babel/plugin-syntax-dynamic-import -D
```

15、react-router

```text
cnpm install react-router-dom -S
cnpm install @loadable/component @types/loadable__component【组件懒加载】 @types/react-router-dom -D
```

16、添加测试

## 错误

1、冻结的语法

```text
[DEP_WEBPACK_COMPILATION_ASSETS] DeprecationWarning: Compilation.assets will be frozen in future, all modifications are deprecated.
```

```text
cnpm html-webpack-plugin@5.0.0-alpha.7  -D
```

