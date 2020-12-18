# 二分法

## 二分查找模板

**前提是需要有序的内容，通过减少比对次数获取内容**

```javascript
let nums = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

function binarySearch(arr, target) {
    let start = 0,
        end = arr.length - 1
    while (start <= end) {
        let mid = (start + end) >> 1
        if (arr[mid] == target) {
            return mid
        } else if (arr[mid] < target) {
            start = mid + 1
        } else {
            end = mid - 1
        }
    }
    return 'no search'
}

console.log(binarySearch(nums, 0))
console.log(binarySearch(nums, 1))
console.log(binarySearch(nums, 2))
console.log(binarySearch(nums, 3))
console.log(binarySearch(nums, 4))
console.log(binarySearch(nums, 5))
console.log(binarySearch(nums, 6))
console.log(binarySearch(nums, 7))
console.log(binarySearch(nums, 8))
console.log(binarySearch(nums, 9))
console.log(binarySearch(nums, 10))
console.log(binarySearch(nums, 11))
```

## 求质数

### 二分

```text

```

### 根号2分

```text

```

## leetcode

### [4. 寻找两个正序数组的中位数](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)

```text

```

### [50. Pow\(x, n\)](https://leetcode-cn.com/problems/powx-n/)

这题的考点是快速幂，就是将大的幂分解为小的幂，然后做到减少计算量的过程

如：求 2的 71 次方

分解如下【可以推断出：两种情况，奇数 `x * x + 2`偶数`x * x`】

2^35 2^35 2^1

2^17 2^17 2^1

2^8 2^8 2^1

2^4 2^4

2^2 2^2

```javascript
var myPow = function (x, n) {
    if (n >= 0) return quickPow(x, n)
    return 1 / quickPow(x, -n)
};

let quickPow = function (x, n) {
    if (n === 0) return 1
    let tempVal = quickPow(x, Math.floor(n / 2))
    return n % 2 === 0
        ? (tempVal * tempVal)
        : (tempVal * tempVal * x)
}
```

### [69. x 的平方根](https://leetcode-cn.com/problems/sqrtx/)

输入8

| left | right | mid |
| :--- | :--- | :--- |
| 1 | 4 | 2 |
| 2 | 4 | 3 |
| 2 | 3 | 终止循环 |

```javascript
var mySqrt = function (x) {
    if (x < 2) return x
    let left = 1
    let right = x >> 1     // 除以2并取整，缩小一下遍历的范围
    while (left + 1 < right) {  // 退出循环时，它们相邻
        let mid = (left + right) >> 1;
        if (mid * mid == x) {
            return mid
        } else if (mid * mid < x) {
            left = mid
        } else {
            right = mid
        }
    }
    // 这种情况是 left + 1 == right
    // 测试用例是8的时候
    // console.log(left, right) // 2 ， 3
    return (right * right > x) ? left : right
};
```

### [~~29. 两数相除~~](https://leetcode-cn.com/problems/divide-two-integers/)

**做不出来**

```text

```

### [278. 第一个错误的版本](https://leetcode-cn.com/problems/first-bad-version/)

**二分法的第一个数：第一个大于目标数的数……**，找到目标之后不中断，截断数据继续找。

```javascript
var solution = function (isBadVersion) {
    return function (n) {
        let left = 1,
            right = n,
            firstBadVersion = n
        while (left <= right) {
            const mid = (right + left) >>> 1
            if (isBadVersion(mid)) {
                firstBadVersion = mid
                right = mid - 1
            } else {
                left = mid + 1
            }
        }
        return firstBadVersion
    }
};
```

### [162. 寻找峰值](https://leetcode-cn.com/problems/find-peak-element/)

> `nums[mid] > nums[mid + 1]` 说明开始下坡，坡顶在左侧，否则坡顶在右侧。

```javascript
var findPeakElement = function (nums) {
    let left = 0,
        right = nums.length - 1;
    // 二分查找
    while (left < right) {
        let mid = (left + right) >> 1
        if (nums[mid] > nums[mid + 1]) {
            right = mid;
        } else {
            left = mid + 1;
        }
    };
    return right;

};
```

