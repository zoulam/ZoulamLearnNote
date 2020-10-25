# note2

## note2

## 1、模块化

common.js ----- AMD

es6 module\(浏览器也难以支持\)

UMD 直接在浏览器使用

### ①typescript项目的编译过程

[commonjs和esMODULE对比](https://www.rollupjs.com/guide/faqs/#%E4%B8%BA%E4%BB%80%E4%B9%88es%E6%A8%A1%E5%9D%97%E6%AF%94commonjs%E6%9B%B4%E5%A5%BDwhy-are-es-modules-better-than-commonjs-modules)

TS\(**TSC**\) =&gt; ES6 modules.js =&gt; \(入口文件引入\)index.tsx =&gt; webpack 实现打包

### ②创建入口文件

需要在每个组件写一个index中转暴露

[pkg.module](https://github.com/rollup/rollup/wiki/pkg.module)

### ③样式文件

[node-sass](https://github.com/sass/node-sass) comand-line-interface

## 2、模块化之后的测试

进入指定文件夹`npm link` 原理是创建快捷方式粘贴到 全局目录下

`npm link <packageName>` 你再package.json写了什么名字就是什么名字

## 3、调试错误

**项目和component library的react版本不一致**

本地：强制使用跟组件相同的`npm link ../xx/node_modules/react`

Publish: 添加配置，在使用库的时候**弹出警告**

```text
  "peerDependencies": {
    "react": ">=16.8.0",
    "react-dom": ">=16.8.0"
  },
```

## 4、本地链接npm账户

npm adduser 注册

npm login 登录

**注**：记得切换 `npm config ls`的仓库地址

![&#x767B;&#x5F55;&#x6D41;&#x7A0B;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201020094541682.png)

### ①package.json配置

[版本规则](http://nodejs.cn/learn/semantic-versioning-using-npm/)

```text
name
version
author
private
license
keywords # npm上的关键字搜索
repository
files # 需要上传到npm的文件 还有一部分是一定回上传的，package.json LICENSE readme.md change.log
```

### ②校正dependencies和devDependencies

优化用户下载包的体积

## 5、Publish前的校正

```javascript
"test:nowatch": "cross-env CI=true react-scripts test", # react-script 是基于开发环境的，所以需要这个声明在publish前也测试一次
```

[eslint命令](https://eslint.org/docs/user-guide/command-line-interface)

### ①流程

运行测试（错误就中断）=&gt; 检查代码规范 =&gt; 代码打包 =&gt; 代码publish

### ②添加husky进行 git commit 的校验

## 6、CI/CD

ci （持续集成）

​ 频繁将代码集成的master

​ 快速发现错误

​ 防止分支大幅偏离主干

cd（持续交付）

​ 频繁将软件的新版本，交付给质量团队或用户

​ 代码通过评审之后，自动部署到生产环境

### ①Travis

[官方地址](https://travis-ci.org/)

​ 需要sync账号，以及配置token

#### 配置 （`.travis.yml`）

**注：**默认使用`yarn`管理，如果不想使用就需要将 `yarn.lock`删除

```text
language: node_js
os: [
    "win"
  ],
node_js:
  - "stable" # nodejs 版本
cache:
  directories:
    - node_modules # 缓存node_modules
env:
  - CI=true # 环境比那辆
script:
  - npm run build-storybook
deploy:
  provider: pages
  skip_cleanup: true
  github_token: $github_token
  # build 之前需要保留
  # keep_history: true # 不适用缓存，每次都重新npm run build-storybook
  local_dir: storybook-static
  on:
    branch: master
```

​ 完成之后就可以在线查看自己的`git commit`的信息了

### ②发布静态页面

[github-pages](https://pages.github.com/)

[发布方式](https://docs.travis-ci.com/user/deployment/pages/)

