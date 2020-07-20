# vscode和win10技巧

## vscode使用技巧

### 1、快捷键篇

下面附上个人常用和改动过的快捷键（Windows操作系统下）：快捷键是帮助加快编写代码，以个人习惯为准。

| 快捷键 | 作用 |
| :--- | :--- |
| `alt+ctrl+m` | 全部大写 |
| `alt+ctrl+l` | 全部小写 |
| `alt+ctrl+；` | 首字母大写 |
| `home` | 行头 |
| `end` | 行尾 |
| `ctrl+home` | 文件头 |
| `ctrl+[` | 删除缩进\(增加缩进直接tab\) |
| `ctrl+enter` | 等价于先end再enter的效果 |
| `ctrl+end` | 文件尾 |
| `ctrl+backspace` | 删除光标前的整个单词 |
| `ctrl+delete` | 删除光标后整个单词 |
| `ctrl+alt+⬇` | 移动到下一个更改点 |
| `ctrl+alt+⬆` | 移动到上一个更改点 |
| `alt+⬇` | 向下移动选定文本一行 |
| `alt+⬆` | 向上移动选定文本一行 |
| `shift+alt+⬇` | 向下复制选定文本一次 |
| `shift+alt+⬆` | 向上复制选定文本一次 |
| `ctrl+[` | 减少缩进 |
| `ctrl+*` | 块注释： /\*\*/  |
| `ctrl+/` | 行注释： // |
| `ctrl+0` | 折叠所有块注释（失效） |
| `ctrl+0+.` | 删除行注释 |
| `ctrl+shift+c` | 打开外部终端 |
| `ctrl+shift+➡` | 向右拆分编辑器 |
| `ctrl+~(在数字1的左边)` | 编辑器和终端之间跳转 |
| `①home+②shift+end` | 选中当前行（分两步操作） |
| `ctrl+d` | 选中一个单词 |
| `shift+delete` | 删除当前行 |
| `shift+alt+鼠标左键` | 光标多行输入 |
| `点击鼠标滚轮` | 也是选定多行输入，有些小区别 |

[官方快捷键打印](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

**注：**不翻墙打开有点慢

### 2、自定义snippet

找到自己的编程语言的json文件，如`JavaScript.json`

![](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200719193459958.png)

> #### 语法简要介绍
>
> 根据个人习惯和喜好设置的快捷输入方式，以提高效率为准，最好先熟悉再使用模板，不然面试的时候就不好过了。
>
> **$1**:定位符光标悬停位置
>
> **prefix**：绑定的代码前缀prefix（可以绑定多个前缀）
>
> **body**：补全的代码内容
>
> **description**：代码描述
>
> **注：**部分符号如 " "无法直接使用需要转义后使用

```javascript
// javascript.json
{
    "JS template1": {
        "prefix": [
            "con",
            "console"
        ], // 对应的是使用这个模板的快捷键
        "body": [
            "console.log($1);",
        ],
        "description": "控制台输出" // 模板的描述
    },
    "JS template2": {
        "prefix": "dw",
        "body": [
            "document.write($1);",
        ],
        "description": "body内输出"
    },
}
```

## Windows使用技巧

### 1.快捷键

| 操作内容 | 快捷方式 |
| :--- | :--- |
| `win+i` | 打开设置 |
| `win+a` | 打开操作中心 |
| `win+k` | 打开连接设备 |
| `win+d` | 显示桌面 |
| `win+shift+s` | 打开windows自带截图 |
| `win+ctrl+d` | 创建虚拟桌面 |
| `win+ctrl+⬅` | 向左切换虚拟桌面 |
| `win+ctrl+→` | 向右切换虚拟桌面 |
| `win+；` | 调出emoji表情 |
| `win+L` | 锁定桌面 |
| `win+v` | 打开云剪贴板（里面包含你开机过程中复制的所有内容） |
| `win+s` | 打开搜索 |
| `win+e` | 打开文件管理器 |
| `win+r` | 打开文件搜索器（输入内容直达文件，一般是win10系统文件比如远程连接，cmd等，其余用户文件用win+s搜索打开体验更佳） |

### 2.常用的dos（cmd）命令

| 命令 | 功能 |
| :--- | :--- |
| `tree /f` | 生成目录树 |
| `md（make directory）` | 创建文件夹 |
| `cd   (change directory)` | 跳转目录 |
| `F:` | 进入F盘 |
| `dir` | 显示目录下的文件 |
| `cd..` | 返回上级目录 |
| `cd/` | 返回根目录 |
| `echo xxxx >.abc.doc` | 创建文件xxxx为文件内容，abc为文件名，doc为文件类型 |
| `del` | 删除 |
| `exit` | 退出dos命令 |
| `del  *.txt` | 删除所有txt类型的文件 |
| `rd（remove directory）` | 删除目录（出于安全考虑，只能删除空目录） |
| `java -version` | 查看java的版本或是否安装jdk |
| `javac HelloWorld.java` | 将java文件编译成字节码（.class）文件 |
| `java HelloWorld  88 “99”` | 运行字节码文件 后面的88是向mian方法传入参数（可以带引号也可以不带，以空格为下个标志） |
| `cls` | 清空控制台 |
| `tab` | 补全文件信息\(这就是我为什么喜欢在文件名上添上数字编号的原因\) |
| `javadoc -d   文件夹名字（自定义）    -author      -version HelloWorld.java` | 生成API文档（index.HTML是文件入口） |
| `java -help` | 查看java命令 |
| `javadoc -help` | 查看javadoc命令 |

### 3.好用的win10插件

[知乎的一个回答](https://www.zhihu.com/question/24584741/answer/1311415699)

提高效率的好工具，希望不是以后不是以插件的形式出现，而且我最喜欢的`PowerToys Run`工具居然不能用在win10家庭版上，只好用`win+s`的快捷键将就一下了

