# env

## 1、平台

Windows & Android

## 2、工具

```
nodejs12+【包含npm、yarn自行安装】
	npm install yarn -g
jdk8 【不支持9以上，自行配置环境变量】
Android Studio
Android SDK
```

![sdk](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201105103828974.png)

## 3、环境变量

```
ANDROID_HOME
G:\Android\SDK
```

![path](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201105104340853.png)

```
%ANDROID_HOME%\platform-tools
%ANDROID_HOME%\emulator
%ANDROID_HOME%\tools
%ANDROID_HOME%\tools\bin
```

## 4、初始化项目

> 命名是有规范的

```
npx react-native init <ProjectName>
# 如: 
npx react-native init CommonApp
```



## 5、启动错误

> 启动流程：
>
> 1、打开Android studio 启动模拟器【这一步**目的是**检查是否安装完整的sdk，出现**下面情况**就启动成功】
>
> ​	如果是`offline`设备是关机状态
>
> ![adb devices](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201105142053314.png)
>
> 2、npx react-native run-android 【首次编译会很慢】
>
> ​	①、启动node.js
>
> ​	②、启动 devices
>
> ​	③、编译、安装、启动应用

**error Failed to launch emulator. Reason: Emulator exited before boot..**

```
启动模拟器失败需要手动启动模拟器
```

**Reason: error in opening zip file**

方式一

```
文件损坏【网络原因导致gradle下载的包不全】
删除指定目录 C:\Users\zoulam\.gradle\wrapper\dists\gradle-6.2-all
重新 npx react-native run-android 会重新安装gradle的包
```

方式二【适合下载超时的】

[下载地址](https://services.gradle.org/distributions/gradle-6.2-all.zip)

![下载地址](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201105134228222.png)

修改配置

```
项目下的Project\android\gradle\wrapper\gradle-wrapper.properties
```

```
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
# distributionUrl=https\://services.gradle.org/distributions/gradle-6.2-all.zip
distributionUrl=file\:///C\:/Users/zoulam/.gradle/wrapper/dists/gradle-6.2-all.zip
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
```

**Task :app:installDebug FAILED**

```
删除打包后的build文件，重新运行
npx react-native run-android
```

<img src="https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201105144418684.png" alt="成功" style="zoom:50%;" />

## 6、模拟器使用夜神模拟器

```
安装夜神模拟器
进入 ..\Nox\bin目录下打开cmd
执行 nox_adb devices 当然你也可用手动运行

再使用Android的 adb 链接夜神的devices
adb connect 127.0.0.1:5037 【后面的端口号视情况而定】
```

### **本地端口号**

`http://127.0.0.1:8081/`

夜神的adb和Android装的adb版本不一致

```
nox_adb version 查看
adb version

位于 SDK\platform-tools 的adb粘贴一份重命名成 nox_adb解决这个问题
```

### adb是什么

>  [adb](https://developer.android.google.cn/studio/command-line/adb?hl=zh-cn)
>
>  ​	**android debug bridge:** 调试桥

```
位于 SDK\platform-tools 下的命令行工具

adb devices
```

**关闭占用的服务端口并重启**

`offline`

```
adb kill-server
adb start-server
```

## 7、真机调试

> 打开开发者模式，设置各种允许

[scrcpy的github仓库](https://github.com/Genymobile/scrcpy)

[scrcpy录屏工具下载](https://github.com/Genymobile/scrcpy/releases)

scrcpy包含大量的操作模拟



