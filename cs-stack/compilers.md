# Compilers

## 资料

### 课程视频来源：

两个是一样的视频，前面的尚未完成（2020/7/23）但是翻译较好，我看的是第二个，机翻加自己的一点理解，也还能接受。

[斯坦福大学CS143国内视频的地址语义分析](https://www.bilibili.com/video/BV1cE411f78c?t=1)

[斯坦福大学CS143国内视频的地址机翻](https://www.bilibili.com/video/BV17K4y147Bz)

### 资源：

#### ①虚拟机

链接：https://pan.baidu.com/s/1B6Pty1Px1uXp7bT69fbvfQ 
提取码：hobe

#### ②ppt

链接：https://pan.baidu.com/s/1TLCHOMpQ20G_8HNNgc2uyA 
提取码：69br

# 0、Course Overview

## 词汇

| 词汇                               | 意思                                                         |
| ---------------------------------- | ------------------------------------------------------------ |
| compilers                          | 编译器                                                       |
| interpreters                       | 解释器                                                       |
| Lexical analysis                   | 词法分析                                                     |
| parse                              | 解析；语法分析                                               |
| code generate                      | 代码生成                                                     |
| optimization                       | 最优                                                         |
| intermediate                       | 中间；过渡                                                   |
| semantics analysis                 | 语义分析                                                     |
| syntax                             | 语法                                                         |
| allocation                         | 分配                                                         |
| top-down                           | 自上而下                                                     |
| bottom-up                          | 自底向上                                                     |
| implementation                     | n. [计] 实现；履行；安装启用                                 |
| SDT（Syntax-directed translation） | [语法制导翻译]([https://baike.baidu.com/item/%E8%AF%AD%E6%B3%95%E5%88%B6%E5%AF%BC%E7%BF%BB%E8%AF%91](https://baike.baidu.com/item/语法制导翻译)) |
| tokenizer                          | 分词器，（编译器中将）                                       |
| tokens                             | 词法单元                                                     |
| assign                             | 分配                                                         |
| inconsistencies                    | 矛盾；前后不一致                                             |
| register                           | 寄存器                                                       |
| rom（read only memory）            |                                                              |
| ram（random access memory）        |                                                              |

> 编译器（compilers）
>
> 解释器（interpreters）
>
> 执行效率比编译器执行或者直接手写的机器码效率低，内存占用大的缺点。
>
> 使用**低级语言**软件开发成本特别高，时间周期长，开发难度大。
>
> 所以在早期出现了software的expense远高于hardware（硬件很贵，但软件更贵）的的问题。
>
> 人们最早使用解释器解决这个效率问题，然后再出现编译器（fotran语言）
>

## 编译过程

词法分析=>解析(关注语言语法层面)

类比：将字母分割成单词，解析句子内容

=>语义分析（类型、作用域规则）

类比：对有歧义的问题：尽量在编译前解决，如语法错误提示，用作用域减少歧义

=>优化（Optimization：对程序转换的集合，目的是为了让程序**占用更少的内存**和**运行更快**、在设备中是体现在**更少的耗电量**、**最少的数据库操作**、**最少的网络访问次数**）

类比：编辑文章，减少文章长度

=>代码生成（这一步可以生成机器码、字节码(在虚拟机上执行的代码)、或者其他编程语言(webassembly)）

![编译的5个过程花费占比](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200723162648181.png)

# 1、Cool: The Course Project

## 三个问题

- 为什么有那么多的编程语言？

![老师的解释](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200723180113548.png)

每个application都有**独特的需求**，做出一个万能的语言必然是臃肿的令人讨厌的，编程领域[没有银弹]([https://zh.wikipedia.org/zh/%E6%B2%A1%E6%9C%89%E9%93%B6%E5%BC%B9](https://zh.wikipedia.org/zh/没有银弹))

matlab/fortran就是为了解决科学家（精度计算、数据可视化……）的一些问题而出现的，

Java开发Android、web应用、大数据……

JavaScript前端、node.js后端……

php后端……

c/c++嵌入式，服务器，操作系统……

object-c开发iosAPP

python机器学习

go后端开发

SQL数据库查询

当然也存在着有些人为了练手而写了编程语言。

- 为什么出现新的编程语言？

1、广泛使用的编程语言变动不会很大（一般都需要兼容旧版本，比如有些应用是使用JDK1.6编译的如果更换新版本的JDK就会造成很大的维护成本）

但是像JavaScript这种"渣渣语言"显然就不适合这个论调，ES6之前的坑简直了。😌

2、新语言是从零开始的，还没能留下**屎山代码**，大家都能轻松学习和考虑是否使用，当**生产力**大于培训和更换语言成本时，就会转向新语言。（懒狗是不会更换语言的）

3、 新语言用来填充旧语言的功能缺陷或空白

4、为了减少程序员的**学习成本**，新语言的语法会跟老语言的语法很相似，最经典的就是Java和C++

- 什么是好的编程语言？

编译型？解释型？强类型？弱类型？函数式？面向对象？面向过程？……

## 准备动手

### cool语言入门

cool语言（OOP）广泛用于实现编译器demo，就一个字快

# 2、Lexical Analysis

# 3、Implementation of Lexical Analysis

# 4、Introduction to Parsing

# 5、Syntax-Directed Translation

# 6、Top-Down Parsing & Bottom-Up Parsing I

# 7、Bottom-Up Parsing II

# 8、Semantic Analysis & Type Checking I

# 9、Type Checking II

# 10、Run-time Environments

# 11、Code Generation

# 12、Operational Semantics

# 13、Intermediate Code & Local Optimization

# 14、Global Optimization

# 15、Register Allocation

# 16、Automatic Memory Management



