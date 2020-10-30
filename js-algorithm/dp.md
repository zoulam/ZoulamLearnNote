# 动态规划

## 动态规划

### 动态规划

### 1、动态规划题目特征

判断一道题能否使用动态规划，**通常**是以下情形

1、求最值（最大或最小）

2、求方案数（骰子求和，背包问题）

3、求可行性问题

更多的判断

数据之间**不可以交换**，如打家劫舍里相邻屋子不能同时抢，如果交换了之后情况就完全不同了

**有方向性**：如棋盘上行走的题目

#### ①动态规划的本质

超级抽象\(**不符合实际**\)

**前置信息：计算机不会主动存储信息。**

计算机有一个简单的加法器，只会做n + 1 的计算， 现在我需要计算机告诉我 8 + 1等于几？

计算机：（1 + 1 + 1 + 1 + 1 + 1 + 1 + 1 ） + 1 = 9

按照人的思维： 8 + 1 = 9 啊，但这是你预先知道了8的值，所以才能快速计算出这个结果，而计算机不会，动态规划就是这个原理

#### ②动态规划优化思路

**用已有空间存储dp信息**：dp有时候不光需要dp数组，还需要辅助数组/数值（temp）

[剑指 Offer 42. 连续子数组的最大和](https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/)

**滑动窗口**：不需要不断扩充数组就能得到最值

骰子的可能、爬楼梯

[推荐刷的题目，有分类](https://leetcode-cn.com/circle/article/NfHhXD/)

## 线性 DP

### [300. 最长上升子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

**dp数组** base case

去除空数组之后，上升子序列的最小值为1，即创建跟输入数组等长的填充1的数组。

**动态转移方程**

当前值获取比**在他前面**他小的值的最优解，再+1，新的最优解比原来的大就替换**dp\[i\]**

```javascript
var lengthOfLIS = function (nums) {
    let n = nums.length
    if (!n) return 0
    let dp = new Array(n).fill(1)
    // 填充1是因为最长子序列的最小值为1
    for (let i = 1; i < n; i++) {
         // 获取自己前面比自己小的个数
        for (let j = 0; j < i; j++) {
            if (nums[j] < nums[i]) {
                // dp[j] + 1 的逻辑：我比你都大了，最长上升子序列肯定在你的基础上+1
                dp[i] = Math.max(dp[i], dp[j] + 1)
            }
        }
    }
    console.log(dp)
    return Math.max(...dp)
};
```

### [1143. 最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/)

`text1 = "abcde", text2 = "ace"`

dp数组打印如下

\[

\[ 0, 0, 0, 0 \],

\[ 0, 1, 1, 1 \],

\[ 0, 1, 1, 1 \],

\[ 0, 1, 2, 2 \],

\[ 0, 1, 2, 2 \],

\[ 0, 1, 2, 3 \]

\]

```javascript
var longestCommonSubsequence = function (text1, text2) {
    let t1Len = text1.length
    let t2Len = text2.length

    let dp = Array.from({ length: t1Len + 1 }, () => new Array(t2Len + 1).fill(0))

    for (let i = 1; i < t1Len + 1; i++) {
        for (let j = 1; j < t2Len + 1; j++) {
            if (text1[i - 1] === text2[j - 1]) {
                dp[i][j] = dp[i - 1][j - 1] + 1
            } else {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1])
            }
        }
    }
    // console.log(dp)
    return dp[t1Len][t2Len]
};
```

### [120. 三角形最小路径和](https://leetcode-cn.com/problems/triangle/)

[图解很棒](https://leetcode-cn.com/problems/triangle/solution/shou-hua-tu-jie-dp-si-lu-120-san-jiao-xing-zui-xia/)

关键是从下至上，只关注相邻两层的结构。 初始化思路：二维数组存储每一层的最优解

```text
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
```

优化思路：用一维数组累计,得到值就覆盖 初始化 \[4,1,8,3\] =&gt; \[7, 6, 10, 3\] =&gt; \[9, 10, 10, 3\] =&gt; \[11, 10, 10, 3\]

```javascript
var minimumTotal = function (triangle) {
    let len = triangle.length
    const dp = new Array(len)
    // 初始化dp数组
    for (let i = 0; i < dp.length; i++) {
        dp[i] = triangle[len- 1][i]
    }
    // 从倒数第二列开始迭代
    for (let i = dp.length - 2; i >= 0; i--) {
        for (let j = 0; j < triangle[i].length; j++) {
            dp[j] = Math.min(dp[j], dp[j + 1]) + triangle[i][j]
        }
    }

    // console.log(dp) // [11, 10, 10, 3]
    return dp[0]
};
```

### [53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)

优化思路，使用没用的nums数组空间存储temp

```javascript
var maxSubArray = function (nums) {
    let temp = nums[0]
    let dp = nums[0]
    for (let i = 1; i < nums.length; i++) {
        if (temp > 0) {
            temp += nums[i]
        } else {
            temp = nums[i]
        }
        dp = Math.max(dp, temp)
    }
    return dp
};
```

### [152. 乘积最大子数组](https://leetcode-cn.com/problems/maximum-product-subarray/)

1、前面获取的最大值  _正数 \|\| 前面获取的最小值_  负数 产生最大值

2、前面获取的最大值  _负数 \|\| 前面获取的最小值_  正数 产生最小值

```javascript
var maxProduct = function (nums) {
    let res = nums[0]
    let min = nums[0]
    let max = nums[0]
    let temp1 = 0, temp2 = 0
    for (let i = 1; i < nums.length; i++) {
        temp1 = min * nums[i]
        temp2 = max * nums[i]
        min = Math.min(nums[i], temp1, temp2)
        max = Math.max(nums[i], temp1, temp2)
        res = Math.max(max, res)
    }
    return res
};
```

### [887. 鸡蛋掉落](https://leetcode-cn.com/problems/super-egg-drop/)（dp+二分）

1、二分法可以获得最优解，但是此处有**鸡蛋数的限制**，若是鸡蛋数只有1，那么只能逐次从最低的楼层开始向上尝试

2、只剩下最后一个鸡蛋前可以一直使用二分，只剩下最优一个鸡蛋，就逐个从最低的楼层开始尝试

#### 好理解的

```JavaScript
var superEggDrop = function (K, N) {
  let dp = Array(K+1).fill(0).map(() => new Array(N+1).fill(0))
  // console.log(dp)

  for (let j = 1; j <=N; j++) {
    for (let i = 1; i <= K; i++) {
      /**
       *二分法   碎了  i-1 j-1 ->下面的     没碎 i j -1  -> 上面的 
       * i-1个鸡蛋j-1次测的楼层 +  i个鸡蛋j-1次测的楼层  + 1
       */
      dp[i][j] = 1 + dp[i-1][j-1] + dp[i][j-1]
      
      if (dp[i][j] >= N) {
        // console.log(dp[i][j], i , j)
        return j
      }
    }
  }
  return N
};
```

#### 优化后的

```JavaScript
var superEggDrop = function (K, N) {
    let dp = Array(K + 1).fill(0)
    let cnt = 0
    while (dp[K] < N) {
        cnt++
        for (let i = K; i > 0; i--) {
            dp[i] = dp[i - 1] + dp[i] + 1
        }
    }
    return cnt
};
```

## [198. 打家劫舍（树形dp）](https://leetcode-cn.com/problems/house-robber/)

```JavaScript
var rob = function (a) {
    let n = a.length;
    if (n == 0) return 0;
    if (n == 1) return a[0];
    if (n == 2) return Math.max(a[0], a[1]);
    let dp = Array.from({ length: n }, () => new Array(2))
    dp[0][0] = 0;
    dp[0][1] = a[0];
    for (let i = 1; i < n; i++) {
        dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1]);
        dp[i][1] = a[i] + dp[i - 1][0];
    }
    let ans = Math.max(dp[n - 1][0], dp[n - 1][1]);
    return ans;
};
```

