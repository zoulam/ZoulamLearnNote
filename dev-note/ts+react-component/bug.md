# bug

# 1、autoComponent点击enter重复搜索

添加状态（isEnter）

# 2、cra的webpack错误

**The react-scripts package provided by Create React App requires a dependency:**

  **"webpack": "4.41.0"**

[解决方式](https://github.com/facebook/create-react-app/issues/7920)

缺少指定依赖： `npm install webpack@4.41.0 -S`

# 3、无法获取最新数据

```typescript
    // ! 无法更新成功：setFileList([_file, ...fileList])
    setFileList(prevList => {
      return [_file, ...prevList]
    })
```

# 4、使用 `<></>`会被识别为 **"Unknown" Component**

改为使用`<div></div>`包裹即可

# 5、 No propTypes defined!

需要导出两次 一次是

 `export const Component` 

`export default Component;`此处分号省略回报错。

# 6、node_modules 下的types错误

[解决方案](https://github.com/facebook/create-react-app/issues/8718)

```
// import type * as PrettyFormat from './types';
import * as PrettyFormat from './types';
```

# 7、Travis失败

目前解决方案：远程`build-storybook` 会失败，本地打包再集成

