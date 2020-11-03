# 数组

## 数组

双指针

动态规划缓存

### [1. 两数之和](https://leetcode-cn.com/problems/two-sum/)

```javascript
var twoSum = function (nums, target) {
    // 1. 构造哈希表
    const map = new Map(); // 存储方式 {need, index}

    // 2. 遍历数组
    for (let i = 0; i < nums.length; i++) {
      // 2.1 如果找到 target - nums[i] 的值
      if (map.has(nums[i])) {
        return [map.get(nums[i]), i];
      } else {
        // 2.2 如果没找到则进行设置
        map.set(target - nums[i], i);
      }
    }
};
```

### [15. 三数之和](https://leetcode-cn.com/problems/3sum/)

1、排序

2、双（三）指针

3、去重 首位、左指针、右指针

```javascript
var threeSum = function (nums) {
    nums.sort((a, b) => a - b)
    let len = nums.length
    if (len < 3 || !nums || nums[0] > 0) return []
    let ans = []
    for (let i = 0; i < len - 2; i++) {
        if (i > 0 && nums[i] === nums[i - 1]) continue
        let left = i + 1,
            right = len - 1
        while (left < right) {
            let sum = nums[i] + nums[left] + nums[right]
            if (sum === 0) {
                ans.push([nums[i], nums[left], nums[right]])
                while (nums[left] === nums[left + 1]) { left++ }
                while (nums[right] === nums[right - 1]) { right-- }
                right--
                left++
            }
            else if (sum > 0) right--
            else if (sum < 0) left++
        }
    }
    return ans
};
```

### [42. 接雨水](https://leetcode-cn.com/problems/trapping-rain-water/)

当前柱子的储水量是左侧最高柱子和右侧最高柱子直接的最小值与当前柱子的高度差

`Math.min(leftMax, rightMax) - curHeight`

```javascript
var trap = function (height) {
    let ans = 0;
    let max = 0;
    let leftMax = [];
    let rightMax = [];
    for (let i = 0; i < height.length; i++) {
        leftMax[i] = max = Math.max(max, height[i]);
    }

    max = 0;
    for (let i = height.length - 1; i >= 0; i--) {
        rightMax[i] = max = Math.max(max, height[i]);
    }

    for (let i = 0; i < height.length; i++) {
        ans += Math.min(leftMax[i], rightMax[i]) - height[i];
    }
    return ans;
};
```

### [20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

1、用栈结构储存左括号

2、遇到右括号

2.1 匹配，出栈

2.2 不匹配，false

3、最后判断stack是否为空 **即是否遗留的左括号**

```javascript
var isValid = function (s) {
    if (s.length % 2 !== 0) return false
    var map = {
        "(": ")",
        "[": "]",
        "{": "}"
    }
    var stack = []
    for (var char of s) {
        if (char in map) stack.push(char); 
        else { 
            if (char != map[stack.pop()]) return false;
        }
    }
    return !stack.length
};
```

### [232. 用栈实现队列](https://leetcode-cn.com/problems/implement-queue-using-stacks/)

1、双栈实现，先反转，再用pop模拟shift，再反转

```javascript
MyQueue.prototype.pop = function () {
    while (this.stack1.length !== 0) {
        this.stack2.push(this.stack1.pop())
    }
    let ans = this.stack2.pop()
    while (this.stack2.length !== 0) {
        this.stack1.push(this.stack2.pop())
    }
    return ans
};
```

### [695. 岛屿的最大面积](https://leetcode-cn.com/problems/max-area-of-island/)

```javascript
var maxAreaOfIsland = function (grid) {
    let x = grid.length, y = grid[0].length
    let max = 0
    for (let i = 0; i < x; i++) {
        for (let j = 0; j < y; j++) {
            if (grid[i][j] == 1) {
                max = Math.max(max, cntArea(grid, i, j, x, y))
            }
        }
    }
    return max
};
// 以(1，1)为原点
// 上（0，1）
// 下（2，1）
// 左（1，0）
// 右（1，2）
let handle = (grid, i, j, x, y) => {
    // 处理越界情况
    if (i < 0 || i >= x || j < 0 || j >= y || grid[i][j] == 0) return 0
    let cnt = 1
    // 清零 避免重复遍历
    grid[i][j] = 0
    cnt += cntArea(grid, i + 1, j, x, y)
    cnt += cntArea(grid, i - 1, j, x, y)
    cnt += cntArea(grid, i, j + 1, x, y)
    cnt += cntArea(grid, i, j - 1, x, y)
    return cnt
}
```

### [384.打乱数组](https://leetcode-cn.com/problems/shuffle-an-array)

洗牌算法类似

1、如何确定自己的算法是真的打乱了

2、

```text

```

## 字符串

### [剑指 Offer 48. 最长不含重复字符的子字符串](https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/)

1、双指针+map

左指针【定位】，无重复的左侧，右指针【探测】，无重复的右侧

出现重复，左指针移动到重复元素的右侧

2、ans元素比对每一次没有重复的最大值

```text
0 1 2 3 4 5 6 7 8 9     g到c有五个（4-0）+1
g f a b c d a b c c               （j-i）+1
      ⬆        ×(重复)
      左指针移动到这
```

```javascript
var lengthOfLongestSubstring = function (s) {
    let i = 0, j = 0, ans = 0, map = new Map()
    while (i < s.length && j < s.length) {
        // 注意此处的边界 i <= map.get(s[j]) 是可以取等于的
        if (map.has(s[j]) && i <= map.get(s[j])) {
            i = map.get(s[j]) + 1
        }
        map.set(s[j], j)
        ans = Math.max((j - i) + 1, ans)
        j++
    }
    return ans
};
```

### [14. 最长公共前缀](https://leetcode-cn.com/problems/longest-common-prefix/)

1、初始化ans为字符串数组的第一个 【是子串的情况就会出现：**子串长度 &lt;= 最短字符串长度**】

2、比对并裁剪，为 `”“`字符串就停止裁剪

```javascript
var longestCommonPrefix = function (strs) {
    if (strs.length === 0) return ""
    let ans = strs[0]
    for (let i = 1; i < strs.length; i++) {
        let j = 0
        for (; j < ans.length && strs[i].length; j++) {
            if (ans[j] != strs[i][j]) break
        }
        ans = ans.substring(0, j)
        if (ans === "") return ""
    }
    return ans
};
```

### [93. 复原IP地址](https://leetcode-cn.com/problems/restore-ip-addresses/)

过滤条件

1、四位ip地址【必须刚好消耗完给定的字符串】

2、除了0之外不能以0开头

3、不能超过255

4、不能越过字符串的长度

```javascript
var restoreIpAddresses = function (s) {
    let res = []
    let hanle = (arr = [], start = 0) => {
        if (arr.length == 4 && start == s.length) {
            res.push(arr.join('.'))
            return
        }
        if (arr.length == 4 && start < s.length) {
            return
        }
        for (let i = 1; i <= 3; i++) {
            // 产品检测 不能越界 不能是0作为起始填充位，不能是>255
            if (start + i - 1 >= s.length) return
            if (i > 1 && s[start] == '0') return
            let temp = s.substring(start, start + i)
            if (i == 3 && +temp > 255) return
            arr.push(temp)
            hanle(arr, start + i)
            arr.pop() // 回溯
        }
    }
    hanle()
    return res
};
```

### [剑指 Offer 58 - I. 翻转单词顺序](https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/)

```javascript
var reverseWords = function (s) {
    // 1、左右两侧的空格
    let newS = s.trim()
    // 2、分割单词
    let arr = newS.split(' ')
    // 反转
    arr.reverse();
    // 过滤空字符串
    let newArr = arr.filter(el => el != '');
    // 还原为字符串
    let ans = newArr.join(' ');
    return ans;
};
```

