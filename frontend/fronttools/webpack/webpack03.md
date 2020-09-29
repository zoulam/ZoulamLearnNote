# webpack03

> 主要是面试题

## 1、webpack的`loader`和 `plugin` 的区别

[详细可以看这个](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/308)

|          | loader                                                    | plugin                                                       |
| -------- | --------------------------------------------------------- | ------------------------------------------------------------ |
| 功能     | loader转换指定类型的模块功能（原生只支持解析 `js、json`） | plugins能够被用于执行更广泛的任务比如打包优化、文件管理、环境注入等 |
| 使用     | 外部引入                                                  | 外部引入，或者使用webpack自带                                |
| 模糊点   | `babel`是`loader`中的`plugins`                            |                                                              |
| 代码实体 | `function`                                                | `class`                                                      |

## 2、loader的加载顺序

由右向左，由下至上

### css和less配置

```javascript
    module: {// 模块
        rules: [//规则
            // test 指定loader文件
            // css-loader 处理@import语法
            //  style-loader 将css插入到head标签中
            // loader为了做到功能单一进行拆分
            // 从右向左执行
            // 用字符串或者对象（可以传入对象类型的options）存储到数组中
            {
                test: /\.css$/,
                use: [
                    {
                        loader: 'style-loader',
                        options: {
                            insert: 'top',//插入位置
                        }
                    },
                    'css-loader'
                ]
            },
            // sass的loader和语法支持：node-sass sass-loader
            // stylus的loader和语法支持：stylus sass-loader
            {
                test: /\.less$/,
                use: [{
                    loader: "style-loader", // creates style nodes from JS strings
                    options: {
                        insert: 'top'
                    }
                },
                {
                    loader: "css-loader" // translates CSS into CommonJS
                },
                // {
                //     loader: "less-loader"
                // },
                {
                    loader: 'less-loader',
                    options: {
                        lessOptions: {
                            strictMath: true,// compiles Less to CSS
                        },
                    }
                }]
            },
        ]
    }
```

## 3、