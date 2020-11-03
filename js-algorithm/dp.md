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

## 经典问题

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

### [354. 俄罗斯套娃信封问题](https://leetcode-cn.com/problems/russian-doll-envelopes/)

排序 + dp

排序的依据

​	1、根据宽排序

​	2、宽相等，则根据高排序

排序之后【对高的数组】使用最长子序列（longest increasing subsequence）的dp

```javascript
var maxEnvelopes = function (envelopes) {
    if (!envelopes.length) return 0;
    let n = envelopes.length;
    envelopes.sort((a, b) => {
        return a[0] === b[0] ? b[1] - a[1] : a[0] - b[0];
    })
    let height = Array.from({ length: n });
    for (let i = 0; i < n; i++) {
        height[i] = envelopes[i][1];
    }
    return lengthOfLIS(height);
};

const lengthOfLIS = (nums) => {
    let piles = 0, n = nums.length;
    let top = [];
    for (let i = 0; i < n; i++) {
        // 要处理的扑克牌
        let poker = nums[i];
        let left = 0, right = piles;
        // 二分查找插入位置
        while (left < right) {
            let mid = Math.floor((left + right) / 2);
            if (top[mid] >= poker)
                right = mid;
            else
                left = mid + 1;
        }
        if (left == piles) piles++;
        // 把这张牌放到牌堆顶
        top[left] = poker;
    }
    // 牌堆数就是 LIS 长度
    return piles;
};
```



## 树形dp

### [198. 打家劫舍](https://leetcode-cn.com/problems/house-robber/)

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

## 区间DP

### [5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)

从中间出发两侧扩大，直到最大。

​	出发点中间的 

​				bab `(i, i)`

​				baab `(i, i + 1)`

```JavaScript
var longestPalindrome = function (s) {
    if (!s || s.length < 2) return s

    let start = 0, end = 0
    let len = s.length

    const handleMidStart = (left, right) => {
        while (right < len && left >= 0 && s[left] == s[right]) {
            left--
            right++
        }
        return right - left - 1
    }

    for (let i = 0; i < len; i++) {
        let len1 = handleMidStart(i, i)
        let len2 = handleMidStart(i, i + 1)
        let maxLen = Math.max(len1, len2)
        if (maxLen > end - start) {
            start = i - ((maxLen - 1) >> 1)
            end = i + (maxLen >> 1)
        }
    }

    return s.substring(start, end + 1)
};
```

## 股票系列

> [js六道股票](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/solution/mai-mai-gu-piao-zui-jia-shi-ji-6dao-ti-jie-by-xi-5/)
>
> [labuladong团灭股票问题](https://github.com/labuladong/fucking-algorithm/blob/master/%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%E7%B3%BB%E5%88%97/%E5%9B%A2%E7%81%AD%E8%82%A1%E7%A5%A8%E9%97%AE%E9%A2%98.md)

### [121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

当前日期的股票价值减去在此之前股票的最低价格

```javascript
var maxProfit = function (prices) {
    let min = prices[0]
    let max = 0
    for (let i = 1; i < prices.length; i++) {
        min = Math.min(min, prices[i])
        max = Math.max(prices[i] - min, max)
    }
    return max
};
```

### [122. 买卖股票的最佳时机 II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)

多次买入卖出

相邻两天是赚钱的就是好的【更像贪心】

```javascript
var maxProfit = function (prices) {
    let ans = 0
    for (let i = 0; i < prices.length - 1; i++) {
        // 只要相邻两天是赚的就直接卖
        let gap = prices[i + 1] - prices[i]
        if (gap > 0) {
            ans += gap
        }
    }
    return ans
};
```

### [123. 买卖股票的最佳时机 III](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/)

> ​	一共有三个状态
>
> ​	买入（重新买入在卖出之后）、卖出（买入之后）、不操作
>
> ​	买入的利润：上一次卖出 - 当前price
>
> ​			最大利润则需要跟前面买入的最大利润做比较
>
> ​	卖出的利润：上一次买入的利润 + 当前price 【买入也是有利润的，前面卖出赚钱了，此次知识小亏】
>
> ​			最大利润则需要跟前面卖出的最大利润做比较
>
> ​	最终利润：最后一天的选择一定是卖出

```JavaScript
----------------------只演示了首次的，所以买入的时候是0-price----------------------------    
for (let i = 1; i < n; i++){
    //卖出时利润：求最大值（上次交易卖出时利润，本次交易卖出时利润）
    profit_out = Math.max(profit_out, profit_in + prices[i]);
    //买入时利润：求最大值（上次买入时利润，本次买入时利润）
    profit_in = Math.max(profit_in,  0 - prices[i]);
}
```

```javascript
var maxProfit = function (prices) {
    let proIn1 = -prices[0], proOut1 = 0
    let proIn2 = -prices[0], proOut2 = 0
    for (let i = 1; i < prices.length; i++) {
        proOut2 = Math.max(proOut2, proIn2 + prices[i])
        proIn2 = Math.max(proIn2, proOut1 - prices[i])
        proOut1 = Math.max(proOut1, proIn1 + prices[i])
        proIn1 = Math.max(proIn1, 0 - prices[i])
    }
    return proOut2
};
```

### [188. 买卖股票的最佳时机 IV](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iv/)

```JavaScript
var maxProfit = function (k, prices) {
    let n = prices.length;
    if (k > n / 2) {
        k = Math.floor(n / 2);  //这样也可以，但其实增加了时间复杂度和内存消耗
        // return maxProfit_k_infinity(prices); //也可以
    }
    let profit = new Array(k);
    //初始化买入卖出时的利润
    for (let j = 0; j <= k; j++) {
        profit[j] = {
            profit_in: -prices[0],
            profit_out: 0
        };
    }
    for (let i = 0; i < n; i++) {
        for (let j = 1; j <= k; j++) {
            profit[j] = {
                profit_out: Math.max(profit[j].profit_out, profit[j].profit_in + prices[i]),
                profit_in: Math.max(profit[j].profit_in, profit[j - 1].profit_out - prices[i])
            }
        }
    }
    return profit[k].profit_out;
}
```

## 字符串系列

### [10. 正则表达式匹配](https://leetcode-cn.com/problems/regular-expression-matching/)

> [图解来源](https://leetcode-cn.com/problems/regular-expression-matching/solution/shou-hui-tu-jie-wo-tai-nan-liao-by-hyj8/)
>
> 情况分析
>
> ​	**注：** 星号只能跟在字母或点后面
>
> 左往右
>
> ![左往右](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/073085fa67286871f76e8e9daa162bdb291a101b4314666c75379a7b0441cad6-image.png)
>
> 右往左
>
> ![右往左](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/5e7b1748039a2a779d7378bebc4926ef3e584e88cc22b67f3a4e18c0590bcc55-image.png)
>
> ​	1、 尾部匹配，剔除尾部继续匹配 `s[i - 1] == p[j - 1]`
>
> ​	2、尾部不匹配 `s[i - 1] !== p[j - 1]`
>
> ​			2.1、正则是 `*`向前拷贝 `s[j - 1] == '*'`
>
> ​					2.1.1、前移一位继续匹配，匹配失败 `s[j - 2] !== s[i - 1]`
>
> ​					2.1.2、前移一位继续匹配，匹配成功  ` s[j - 2] == s[i - 1]`
>
> ​											①不剔除尾部：前面字符串继续出现 **（baaaa 和 ba*）**
>
> ​													`i -`		表示字符串移动， `j -`表示正则移动
>
> ​													`dp[i][j] = dp[i - 1][j]`
>
> ​											②剔除尾部：字符串只出现一次 **（ba 和 ba*）**
>
> ​													`dp[i][j] = dp[i - 1][j - 2]`
>
> ​											③剔除尾部：字符串没有出现 **（bac 和 baca*）**
>
> ​													`dp[i][j] = dp[i][j - 2]` 
>
> ​			2.2、不是 `*`号 匹配失败 `s[j - 1] == '*'`
>
> dp思路
>
> ​	1、创建一个二维数组，初始全部为`false`
>
> ​	2、将 `[0][0]`位置的初值设置为 `true`
>
> ​	3、当前正则的前面是 `*`对应的`dp[0][j] = dp[0][j - 2]` ，子串情况是前面两位的子串，假设 `*`指代出现次数为0。

```javascript
var isMatch = function (s, p) {
    if (s == null || p == null) return false;
    let sLen = s.length, pLen = p.length
    let dp = Array.from({ length: sLen + 1 }, () => new Array(pLen + 1).fill(false))
    dp[0][0] = true
    for (let i = 1; i < pLen + 1; i++) {
        if (p[i - 1] == '*') dp[0][i] = dp[0][i - 2]
    }

    for (let i = 1; i < sLen + 1; i++) {
        for (let j = 1; j < pLen + 1; j++) {
            if (s[i - 1] == p[j - 1] || p[j - 1] == '.') {
                dp[i][j] = dp[i - 1][j - 1]
            } else if (p[j - 1] == '*') {
                if (s[i - 1] == p[j - 2] || p[j - 2] == '.') {
                    dp[i][j] = dp[i - 1][j - 2] || dp[i - 1][j] || dp[i][j - 2]
                } else {
                    dp[i][j] = dp[i][j - 2]
                }
            }
        }
    }

    return dp[sLen][pLen]
};
```

