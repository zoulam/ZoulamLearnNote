# \[cpp\]-env

## 环境配置（vscode）

### 安装插件

![&#x63D2;&#x4EF6;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200726143022710.png)

下面的两个文件都会出现在.vscode文件夹下

### 安装`mingw-w64`

[mingw-w64](https://sourceforge.net/projects/mingw/files/latest/download)

下载编译器和debug的工具，注意都是在**bin**class下的总共四个

`gcc`编译c代码

`g++`编译c++代码

`gdb`debug

`mingw32-make`在：`All Packages -> MinGW Base System -> mingw32-base-bin`目录下

> ①批量编译c++代码，建议改名`make`方便输入
>
> ②命令`gcc *.c -o run`区别：
>
> 不会编译没有变动的文件，可以有较快的编译速度。

![c&#x7F16;&#x8BD1;&#x5668;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200726145923975.png)

下载完成之后applychange

![&#x4FDD;&#x5B58;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200726150255042.png)

![&#x914D;&#x7F6E;&#x73AF;&#x5883;&#x53D8;&#x91CF;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200726150634967.png)

在`Path`中填入

![&#x73AF;&#x5883;&#x53D8;&#x91CF;2](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200726150749798.png)

配置成功后

```bash
gcc --version
g++ --version
```

![&#x6210;&#x529F;&#x540E;&#x7684;&#x7ED3;&#x679C;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200726150940559.png)

### 配置编译任务`task.json`

`ctrl+shft+b`打开

![&#x5165;&#x53E3;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200726143422356.png)

```javascript
{
  // See https://go.microsoft.com/fwlink/?LinkId=733558
  // for the documentation about the tasks.json format
  "version": "2.0.0",
  "tasks": [
    {

      "type": "shell",
      "label": "c++ compiler", //这里注意一下，见下文
      "command": "G:\\developer_tools\\cpp\\MinGW\\bin\\g++.exe",
      "args": [
        "-g",
        "${file}",
        "-o",
        "${fileDirname}\\output\\${fileBasenameNoExtension}.exe",
        "-ggdb3", // 生成和调试有关的信息
        "-Wall", // 开启额外警告
        "-static-libgcc", // 静态链接
        "-std=c++17", // 使用c++17标准
        "-finput-charset=UTF-8", //输入编译器文本编码 默认为UTF-8
        "-fexec-charset=GB18030", //输出exe文件的编码
        "-D _USE_MATH_DEFINES"
      ],
      "options": {
        "cwd": "G:\\developer_tools\\cpp\\MinGW\\bin"
      },
      "problemMatcher": ["$gcc"],//匹配到gcc程序
      "presentation": {
        "echo": true,
        "reveal": "always", // 在“终端”中显示编译信息的策略，可以为always，silent，never
        "focus": false,
        "panel": "shared" // 不同的文件的编译信息共享一个终端面板
      },
      "group": "build"//设置分组，将生成目录和编译两件事情放在一起
    },
    // 创建output
    {
      "type": "shell",
      "command":"mkdir",
      "args": ["ouput"],
      "label": "mkdir",
      "group": "build"
    },
    //运行代码
    {
      "label": "cd",
      "type": "shell",
      "command":"cd",
      "args": [" output"]
    },
    //组合任务
    {
      "label": "run code",
      "dependsOrder": "sequence",//顺序执行dependsOn的内容
      "dependsOn":["mkdir","c++ compiler","run"],//组合便签
    }
  ]
}
```

### debug的`launch.json`

对代码页面摁`F5`

![](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200726151523328.png)

![](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200726151559087.png)

```javascript
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "cpp debug", // 配置名称，将会在启动配置的下拉菜单中显示
      "type": "cppdbg", // 配置类型，这里只能为cppdbg
      "request": "launch", // 请求配置类型，可以为launch（启动）或attach（附加）
      "program": "${fileDirname}/output/${fileBasenameNoExtension}.exe", // 将要进行调试的程序的路径
      "args": [], // 程序调试时传递给程序的命令行参数，一般设为空即可
      "stopAtEntry": false, // 设为true时程序将暂停在程序入口处，一般设置为false
      "cwd": "${fileDirname}/output", // 调试程序时的工作目录，一般为${workspaceFolder}即代码所在目录
      "environment": [],
      "externalConsole": true, // 调试时是否显示控制台窗口，一般设置为true显示控制台
      "MIMode": "gdb",
      "miDebuggerPath": "G:\\developer_tools\\cpp\\MinGW\\bin\\gdb.exe", // miDebugger的路径，注意这里要与MinGw的路径对应
      "preLaunchTask": "c++ compiler", // 调试会话开始前执行的任务，一般为编译程序，c++为g++, c为gcc
      "setupCommands": [
        {
          "description": "Enable pretty-printing for gdb",
          "text": "-enable-pretty-printing",
          "ignoreFailures": true
        }
      ]
    }
  ]
}
```

### `c_cpp_properties.json`关联编译器路径和IntelliSense设置

```javascript
{
    "configurations": [
        {
            "name": "MinGW64",
            "intelliSenseMode": "gcc-x64",
            "compilerPath": "G:\\developer_tools\\cpp\\MinGW\\bin\\g++.exe",
            "includePath": [
                "${workspaceFolder}"
            ],
            "cppStandard": "c++17"
        }
    ],
    "version": 4
 }
```

**注：**每次都要在该文件目录下手动建立`output`文件夹，当然你也可以使用`Task`中的`group`来完成这些事情

## makefile的基本使用技巧

**注：**下方是使用c编译器做的示范

```text
#一次性编译和链接
# app: main.c fun1.c fun2.c
#     gcc main.c fun1.c fun2.c -o app


# 使用-c参数，编译出中间文件xx.o但是不链接，最后再对xx.o文件进行链接。
# 好处：c代码文件没有变动就不用重新编译，即：只重新编译变化的c代码文件再链接
# app: main.o fun1.o fun2.o
#     gcc main.o fun1.o fun2.o -o app
# main.o: main.c
#     gcc -c main.c -o main.o
# fun1.o: fun1.c
#     gcc -c fun1.c -o fun1.o
# fun2.o: fun2.c
#     gcc -c fun2.c -o fun2.o


# 三个常用自动变量
# $<：第一个依赖文件；
# $@：目标；
# $^：所有不重复的依赖文件，以空格分开
# obj = main.o fun1.o fun2.o
# target = app
# CC = gcc
# #执行上方传入的参数
# $(target): $(obj)
#     $(CC) $(obj) -o $(target)
# #所有.c文件编译成.o
# %.o: %.c
#     $(CC) -c $< -o $@

# 使用函数简化操作，一次性编译当前文件夹下的c代码
# wildcard:通配操作函数
# patsubst:通配替换操作函数
# src = $(wildcard ./*.c)#此处示例的返回值是：main.c fun1.c fun2.c
# #将.c文件名替换成编译后的.o文件名
# obj = $(patsubst %.c, %.o, $(src))#此处示例的返回值是：main.o fun1.o fun2.o
# # obj = $(src:%.c=%.o)#与上一行的obj效果相同
# target = app
# CC = gcc

# $(target): $(obj)
#     $(CC) $(obj) -o $(target)
# %.o: %.c
#     $(CC) -c $< -o $@

# # 对临时生成的.o文件进行清除，使用make clean即可
# .PHONY:clean
# clean:
#     #Windows操作系统
#     del  *.o *.exe
#     #mac or linux
#     # rm -rf $(obj) $(target)


src = $(wildcard ./*.c)
obj = $(src:%.c=%.o)
target = app
CC = gcc
$(target): $(obj)
    $(CC) $(obj) -o $(target)
%.o: %.c
    $(CC) -c $< -o $@

.PHONY:clean
clean:
    del  *.o
    @echo "----------clean completed--------"

.PHONY:del
del:
    del  $(target).exe
    @echo "----------clean project----------"

# .PHONY:getdir
# getdir:
#     mkdir output
#     cd output
```

