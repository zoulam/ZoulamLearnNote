# \[cpp\]-env

# Ⅰvscode

## 1、安装 MinGW [下载地址](https://sourceforge.net/projects/mingw/files/latest/download)

​	**注：** MinGW是一种软件包管理工具，不包含c++的编译器

## 2、使用MinGW安装编译器

​	点击 `All Packages`

​	安装：`gcc`c编译器,`g++`c++编译器,`gdb`：debug工具

​	他们分别对应的文件名是

```
mingw-32-gcc-bin
mingw-32-gcc-g++-bin
mingw-32-gdb-bin
```

勾选文件名之后，`Installation` -> `Apply Changes`

安装完成后**检查**：

​	你之前的 **MinGW**安装在哪他就在哪，   `...\MinGW\bin`下查看是否安装成功

## 3、配置环境变量

​	变量名：MinGW

​	变量值：...\MinGW

再添加环境变量

```
%MinGW%\bin
```

配置完成后**检查**：

命令行输入：`gcc -v`，`g++ -v` `gdb -v` 查看。

## 4、配置vscode

### 4、1安装vscode

​	略

### 4、2安装c/c++插件

​	略

### 4、3运行helloworld

```cpp
#include<iostream>
using namespace std;
int main(){
    cout << "hello echo" << endl;
   	system("pause");
}
```

### 4、4不用插件运行

创建文件夹（`.vscode`）和文件

```bash
├─.vscode
│      c_cpp_properties.json
│      launch.json
│      tasks.json
# 下面两个文件可以不需要，
│      settings.json
│      cpp.code-workspace
```

我的配置

#### launch.json

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "cpp debug", // 配置名称，将会在启动配置的下拉菜单中显示
      "type": "cppdbg", // 配置类型，这里只能为cppdbg
      "request": "launch", // 请求配置类型，可以为launch（启动）或attach（附加）
      // "program": "${fileDirname}/output/${fileBasenameNoExtension}.exe", // 将要进行调试的程序的路径
      "program": "${fileDirname}/${fileBasenameNoExtension}.exe", // 将要进行调试的程序的路径
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

#### task.json

```json
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558 
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
        {
            "type": "shell",
            "label": "g++", //这里注意一下，见下文
            "command": "D:\\Environment\\MinGW\\bin\\g++.exe",
            "args": [
                "-g",
                "${file}",
                "-o",
                "${fileDirname}\\${fileBasenameNoExtension}.exe",
                "-ggdb3",   // 生成和调试有关的信息
                "-Wall",    // 开启额外警告
                "-static-libgcc",   // 静态链接
                "-std=c++17",       // 使用c++17标准
                "-finput-charset=UTF-8",    //输入编译器文本编码 默认为UTF-8
                "-fexec-charset=GB18030",   //输出exe文件的编码
                "-D _USE_MATH_DEFINES"
            ],
            "options": {
                "cwd": "D:\\Environment\\MinGW\\bin"
            },
            "problemMatcher": [
                "$gcc"
            ],
            "presentation": {
                "echo": true,
                "reveal": "always", // 在“终端”中显示编译信息的策略，可以为always，silent，never
                 "focus": false,
                 "panel": "shared" // 不同的文件的编译信息共享一个终端面板
            },
        }
    ]
}
```

#### c_cpp_properties.json

```json
{
    "configurations": [
        {
            "name": "MinGW64",
            "intelliSenseMode": "gcc-x64",
            "compilerPath": "G:\\developer_tools\\cpp\\MinGW\\bin\\g++.exe",// g++编译器路径
            "includePath": [
                "${workspaceFolder}"
            ],
            "cppStandard": "c++17"
        }
    ],
    "version": 4
 }

```

点击 `F5`即可运行程序

注： **还不能模块化编译**

参考文章：

​	[Configuring C/C++ debugging](https://code.visualstudio.com/docs/cpp/launch-json-reference)

​	[Integrate with External Tools via Tasks](https://code.visualstudio.com/docs/editor/tasks)

# Ⅱ模块化编译

## 1、makefile

**注：**运行makefile文件的方式是在终端的同级目录下运行： `make`命令

### 1、1固定编译

```makefile
#逐行执行,命令是以Tab开始

#目标文件：源文件
main:main.o my.o
# gcc 编译函数  -o 指定生成的文件名
	gcc main.o my.o -o main
main.o:main.c
	gcc -c main.c
my.o:my.c
	gcc -c my.c

.PHONY:clean
#自定义函数clean 删除.o结尾的文件 使用make clean可以调用
clean:
	@echo "=======clean project========="
	del  *.o
	@echo "=======clean completed========="
```

1、2

## 2、cmake

### 2、1入门前先看

[如何评价 CMake？](https://www.zhihu.com/question/276415476)

[CMake 如何入门？](https://www.zhihu.com/question/58949190/answer/999701073)

# Ⅲ包管理工具



# linux