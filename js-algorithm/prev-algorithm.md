---
description: 温馨提示，本文是以JavaScript为主要语言写的数据结构与算法，以及刷leetcode的记录
---

# 前置知识

## 熟记常用API

### `Math`

`Math.max(value)`

`Math.min(value)`

`Math.floor(value)` 向下取整\(即取更小的值\) 45.9=&gt;45 -45.9=&gt;-46

`Math.sqrt(value)`

`Math.pow(底数、指数)` 或 `底数 ** 指数`

`Math.round(value)` 四舍五入

`Math.trunc(value)` 抹零

`Math.abs(value)`

`1e9+7` 常用于取模的超大数

`parseFloat(String)` `parseInt(String,进制格式默认值是10)` `Float.fixed(保留小数的位数)`

`let num = 13.4545` `num.toPrecision(3)` **13.4** 保留指定位数的小数

```javascript
let num = new Number(13.15487)
console.log(num.toPrecision(3));
```

### `String`

`replace(选定内容, 替换内容)` 选定内容常使用 正则填充

`trim()`

`trimRight()`

`trimeLeft()`

`split(分割值)`

`slice(startIndex, endIndex + 1)`

`indexOf()`

`includes()`

`charAt()` 不常用 一般使用语法： `String[index]`

`charCodeAt()`

`toLowerCase()`

`toUpperCase()`

### `Array`

原地操作

`push()` `unshitft()`

`pop()` `shift()`

`reserve()` `sort()`

`splice(startIndex, deleteLength, [...addValue])`

`deleteLength = 0` 时插入 其他从 startIndex位置开始删除

`forEach((el, index, array)=>{do something}, this)`

非原地操作

`slice()` `join()` `toString()` `fill(替换内容, startIndex ,endIndex + 1)`

`concat(a1, a2, a3, ……)` 将后面的数组合并在前面的数组

`map((el, index, array)=>{do something}, this)`

`filter((el, index, array)=>,this)`

表达式中的返回值为true就push进入新数组，false则被过滤

`find(el => el >10 )`

与`filter`的逻辑相同，是短路操作，即只返回一次值

`reduce((start, el, index, array)=> start + el,init)`

这里是一个简单的累加器 `start`为上一次执行的`return`值，`init`是首次进入函数的初始值

`indexOf(value)` `value`存在返回`0` 否则返回`-1`

`includes(value)` 返回值的下标

关于迭代 使用 `for(let value of Array) {}` 才能保证顺序

### `RegExp`

略

### `Map`

>  存储键值对，跟数组的 数组值对应数组下标类似，但存储的信息更加丰富

`set()`

`get()` 取出来查看，并不会删除

`has()`

`clear()`

`delete()`

`size`

### `Set`

`add()`

`get()`

`has()`

`size` 是去重之后的尺寸，而不是填入的长度

`clear()`

了解常用数据结构：队列、优先级队列（`push`、`shift`）、栈\(`push`、`pop`）、链表、双向链表、环形链表、二叉树

二分查找模板、双指针模板、位运算使用场景、

## 花式指针

```JavaScript
// 头尾指针
for(let i = 0; i < nums.length; i++)
for(let i = nums.length - 1; i >=0; i--)

// 相邻指针
for(let i = 0; i < nums.length - 1; i++){
  	  for(let j = i + 1; j < nums.length; j++)
}

// 链表指针
let head = new ListNode(-1)
// 三类指针【当前面需要使用到指针覆盖后面需要指针迭代就需要缓存】
let prev = null
let cur = head
let next = head.next
while(node){
    let val = node.val
    prev = curr
    curr = curr.next 
}
    
// 快慢指针    
    
fast = head    
slow = head 
    fast = fast.next.next
    slow = slow.next

```



## [DFS、BFS](https://zhuanlan.zhihu.com/p/24986203)

DFS深度优先搜索：递归，再回溯（不会走完全部）

 回溯，在约束条件下，会穷举所有节点，通常用于解决「找出所有可能的组合」问题。

BFS广度优先搜索：记录信息（会记录全部）

## 关于树的递归

[leetcode下的总结](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/comments/)

* 写出结束条件（递归的出口）
* 不要把树复杂化，就当做树是三个节点，根节点，左子节点，右子节点（复杂问题简单化）
* 只考虑当前做什么，不用考虑下次应该做什么
* 每次调用应该返回什么

```java
public int minDepth(TreeNode root) {
        if(root==null)
            return 0;
    // 都是空的
        if(root.left==null || root.right==null)
            return 1;
    // 都不是空的
        if(root.left!=null && root.right!=null)
            return Math.min(minDepth(root.left),minDepth(root.right))+1;
    // 左不是空
        if(root.left!=null)
            return minDepth(root.left)+1;
    // 右不是空
        if(root.right!=null)
            return minDepth(root.right)+1;
    // 
        return 1;
    }
```

## 关于树的迭代

> 技巧：创建栈或者队列，逐层遍历

三个节点的最简二叉树情况

1、`root == null`

`if(!root)`

2、`root !== null`

3、 `root.left == null` `root.right !== null`

`else if(root.right)`

4、`root.left !== null` `root.right == null`

`else if (root.left)`

5、`root.left == null` `root.right == null`

`else`

6、`root.left !== null` `root.right !== null`

`if(root.left && root.right)`

## 动态规划思路

赋初值

缓存前面已知的最优解

写状态转移方程

## 排序算法

不稳定的算法：**快**（快排）**些**（希尔）**选**（选择）**一堆**（堆排）

稳定的算法：插入排序、冒泡排序、归并排序

或者

不稳定：快选堆希

稳 定：插冒归基

