# 二叉树

## 二叉树

### 二叉树

### 关于递归的思路

1、将节点简化为（根 左 右）3个节点，只考虑当前行为，

**思考方式**假设以下情况，在根据情况执行递归的的走势

①root为空

②left为空 right不为空

③right为空 left不为空

④right和left都是空

⑤right和left都存在

**一定不要忘记写递归的终止条件**

2、回溯

1、将栈空间的先存入当前数组；

2、数组pop出来返回上一种情况继续讨论分支

### 1、遍历

使用迭代的方式遍历的套路：

1、先判断是否为空，为空直接返回

2、将数据按照特定顺序将**节点**插入的指定数据结构中

3、逐个取出**val**

#### 递归（通用）

> 改变handle防守的遍历顺序即可

```javascript
var preorderTraversal = function (root) {
    if (!root) return [];
    let ans = []
    let handle = (node) => {
        if (node) {
            ans.push(node.val)
            handle(node.left)
            handle(node.right)
        }
    }
    handle(root)
    return ans
};
```

#### 前序遍历（stack）

```javascript
var preorderTraversal = function (root) {
    if (!root) return []
    let stack = [root]
    let ans = []
    while (stack.length > 0) {
        let node = stack.pop()
        ans.push(node.val)
        node.right && stack.push(node.right)
        node.left && stack.push(node.left)
    }
    return ans
};
```

#### 中序遍历\(stack\)

将一旦遇到左节点全部push进去，没有就回溯右节点，再将右节点的全部push到栈中

```javascript
var inorderTraversal = function (root) {
    if (!root) return []
    let stack = [],
        ans = []
    while (root) {
        stack.push(root)
        root = root.left
    }

    while (stack.length > 0) {
        let node = stack.pop()
        ans.push(node.val)
        node = node.right
        while (node) {
            stack.push(node)
            node = node.left
        }
    }
    return ans
};
```

#### 后序遍历\(stack\)

后续遍历的顺序是：左右根

换成前插：1、根

2、右根

3、左右根

```javascript
const postorderTraversal = (root) => {
    if (!root) return []
    let ans = []
    let stack = [root]

    while (stack.length > 0) {
        let node = stack.pop()
        ans.unshift(node.val)
        node.left && stack.push(node.left)
        node.right && stack.push(node.right)
    }
    return ans
};
```

#### BFS层序遍历\(queue\)

逐层将节点push进对象，左先遍历就先push左树，取的时候shift\(\)

```javascript
var levelOrder = function (root) {
    if (!root) return []
    let ans = []
    let queue = [root]
    while (queue.length > 0) {
        let node = queue.shift()
        ans.push(node.val)
        node.left && queue.push(node.left)
        node.right && queue.push(node.right)
    }
    return ans
};
```

#### BFS和DFS

[不错的文章](https://zhuanlan.zhihu.com/p/24986203)

迷宫情况

【单条可行路径】dfs是撞墙就返回，回溯 **栈结构**

【全部路径】bfs是会将全部节点遍历完的 **队列结构**

### 2、深度

#### [重建二叉树（前序和中序）](https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/)

1、 前序的第一个节点就是**root**

2、借此拆分中序的左树和右树，递归处理

```javascript
var buildTree = function (preorder, inorder) {
    if (!preorder.length || !inorder.length) return null;
    let rootVal = preorder[0];
    let node = new TreeNode(rootVal);
    let i = 0;
    for (; i < inorder.length; ++i) {
        if (rootVal === inorder[i]) break;
    }
    // i = 1 左树是9 
    // i = 1 右树是20 15 7
    node.left = buildTree(preorder.slice(1, i + 1), inorder.slice(0, i + 1));
    node.right = buildTree(preorder.slice(i + 1), inorder.slice(i + 1));
    return node;
};
```

#### [106. 从中序与后序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

```javascript
var buildTree = function (inorder, postorder) {
    if (inorder.length == 0 || postorder.length == 0) return null
    let pLen = postorder.length
    let rootVal = postorder[pLen - 1]
    let node = new TreeNode(rootVal)
    let i = 0
    for (; i < pLen; i++) {
        if (rootVal === inorder[i]) break
    }
    node.left = buildTree(inorder.slice(0, i), postorder.slice(0, i))
    node.right = buildTree(inorder.slice(i + 1, pLen), postorder.slice(i, pLen - 1))
    return node
};
```

#### [剑指 Offer 54. 二叉搜索树的第k大节点](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)

```javascript
var kthLargest = function (root, k) {
    let stack = [root]
    let arr = []
    while (stack.length > 0) {
        let node = stack.pop()
        arr.push(node.val)
        node.right && stack.push(node.right)
        node.left && stack.push(node.left)
    }
    arr.sort((a, b) => b - a)
    return arr[k - 1]
};
```

#### [637. 二叉树的层平均值](https://leetcode-cn.com/problems/average-of-levels-in-binary-tree/) 【层序】

**len 必须缓存不然shift之后就改变了**

```javascript
var averageOfLevels = function (root) {
    let ans = []
    let queue = [root]
    while (queue.length > 0) {
        let len = queue.length
        let sum = 0
        // 一层的一次性添加进去
        for (let i = 0; i < len; i++) {
            let node = queue.shift()
            sum += node.val
            node.left && queue.push(node.left)
            node.right && queue.push(node.right)
        }
        ans.push(sum / len)
    }
    return ans
};
```

#### [1302. 层数最深叶子节点的和](https://leetcode-cn.com/problems/deepest-leaves-sum/) 【层序】

```javascript
var deepestLeavesSum = function (root) {
    if (!root) return 0
    let ans = []
    let queue = [root]
    while (queue.length > 0) {
        let len = queue.length
        sum = 0
        for (let i = 0; i < len; i++) {
            let node = queue.shift()
            sum += node.val
            node.left && queue.push(node.left)
            node.right && queue.push(node.right)
        }
        ans.push(sum)
    }
    return ans[ans.length - 1]
};
```

#### [剑指 Offer 55 - I. 二叉树的深度](https://leetcode-cn.com/problems/er-cha-shu-de-shen-du-lcof/)

**递归**

```javascript
var maxDepth = function (root) {
   if (!root) return 0
   return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1
};
```

**非递归【层序遍历】**

**层序遍历**

```javascript
var maxDepth = function (root) {
    if (!root) return 0
    let ans = []
    let queue = [root]
    while (queue.length > 0) {
        // len 必须缓存不然shift之后就改变了
        let len = queue.length
        for (let i = 0; i < len; i++) {
            let node = queue.shift()
            node.left && queue.push(node.left)
            node.right && queue.push(node.right)
        }
        ans.push(0)
    }
    return ans.length
};
```

#### [111. 二叉树的最小深度](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)

1、没有根节点 0

**注：**下面的写法互补的，如果不按照顺序则需要

将 `else if (root.left)` 改为 `else if (root.left && !root.right)`

2、他们后面的+1是加的根节点

2.1、左节点和右节点都有 （**左节点最短的**和**右节点最短的**）中比较小的+1

2.2、只有根节点和右节点 **右节点最短的** +1

2.3、只有根节点和左节点 **左节点最短的** + 1

3、只有根节点 1

```javascript
var minDepth = function (root) {
    if (!root) return 0
    // 两个都不为空
    if (root.left && root.right) {
        return Math.min(minDepth(root.left), minDepth(root.right)) + 1
        // 左不空
    } else if (root.left) {
        return minDepth(root.left) + 1
        // 右不空
    } else if (root.right) {
        return minDepth(root.right) + 1
        // 全空
    } else {
        return 1
    }
};
```

#### [783. 二叉搜索树节点最小距离](https://leetcode-cn.com/problems/minimum-distance-between-bst-nodes/)

**中序遍历能保证二叉搜索树按照从小到大的顺序排列**

```javascript
var minDiffInBST = function (root) {
    if (!root) return 0
    let stack = []
    let ans = []
    while (root) {
        stack.push(root)
        root = root.left
    }

    while (stack.length > 0) {
        let node = stack.pop()
        ans.push(node.val)
        node = node.right
        while (node) {
            stack.push(node)
            node = node.left
        }
    }
    let len = ans.length
    let min = 1e9 + 7
    for (let i = 0; i < len - 1; i++) {
        min = Math.min(min, (ans[i + 1] - ans[i]))
    }
    return min
};
```

#### [剑指 Offer 55 - II. 平衡二叉树](https://leetcode-cn.com/problems/ping-heng-er-cha-shu-lcof/)

1、创建一个获取树深度的辅助函数

2、对树的每一个左右节点比对深度

```javascript
function isBalanced(root) { 
    if (!root) return true
    let leftLen = TreeDepth(root.left)
    let rightLen = TreeDepth(root.right)
    if (Math.abs(leftLen - rightLen) > 1) return false
    return isBalanced(root.left) && isBalanced(root.right)
}

function TreeDepth(node) {
    if (node == null) return 0
    return Math.max(TreeDepth(node.left), TreeDepth(node.right)) + 1
}
```

#### [199. 二叉树的右视图](https://leetcode-cn.com/problems/binary-tree-right-side-view/)

思路：层序遍历把最后面那个元素push进去

```javascript
var rightSideView = function (root) {
    if (!root) return []
    let queue = [root]
    let ans = []
    while (queue.length > 0) {
        let len = queue.length
        for (let i = 0; i < len; i++) {
            let node = queue.shift()
            if (i == len - 1) ans.push(node.val)
            if (node.left) queue.push(node.left)
            if (node.right) queue.push(node.right)
        }
    }
    return ans
};
```

#### [103. 二叉树的锯齿形层次遍历](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)

```javascript
var zigzagLevelOrder = function (root) {
    if (!root) return []
    let queue = [root]
    let ans = []
    let flag = false
    while (queue.length > 0) {
        let len = queue.length
        let temp = []
        for (let i = 0; i < len; i++) {
            let node = queue.shift()
            temp.push(node.val)
            node.left && queue.push(node.left)
            node.right && queue.push(node.right)
        }
        if (flag) {
            temp = temp.reverse()
        }
        flag = !flag
        ans.push(temp)
    }
    return ans
};
```

### 3、路径

#### [112. 路径总和](https://leetcode-cn.com/problems/path-sum/)

思路：

1、传入空节点就终止递归

2、传入**没有子节点的节点**，就最后做一次减法

```javascript
var hasPathSum = function (root, sum) {
    if (!root) return false
    if (!root.right && !root.left) {
        return sum - root.val == 0
    }
    return hasPathSum(root.left, sum - root.val)
        || hasPathSum(root.right, sum - root.val)
};
```

### [236. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

递归思路

1、假设只有三个节点【三个抽象节点，里面包含大量子节点】

![&#x62BD;&#x8C61;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201028000536074.png)

2、**目标节点**在root 直接返回root ，没有root返回null

3、目标节点在left或者right，返回root

4、left为空说明只能在右树向右探索，求最近从root下移，right为空同理

```javascript
var lowestCommonAncestor = function (root, p, q) {
    if (!root) return null
    if (root == p || root == q) return root
    const left = lowestCommonAncestor(root.left, p, q)
    const right = lowestCommonAncestor(root.right, p, q)
    if (!left) return right
    if (!right) return left
    return root
};
```

## 图

### [207. 课程表](https://leetcode-cn.com/problems/course-schedule/)

> [图解来源](https://leetcode-cn.com/problems/course-schedule/solution/bao-mu-shi-ti-jie-shou-ba-shou-da-tong-tuo-bu-pai-/)
>
> ​ A -&gt; B A有一个出度，B有一个入度
>
> ​ 1、入度为0可以直接选， 0 ，1 ，2 已选课程为3
>
> ​ 2、后续导致3、4入度为0，可选课程3、4，已选课程`3 + 2 = 5`
>
> ​ 3、5入度为0可选，可选课程5，已选课程`5 + 1 = 6`
>
> ​ ![&#x5FAE;&#x4FE1;&#x622A;&#x56FE;\_20200517052852.png](https://pic.leetcode-cn.com/de601db5bd50985014c7a6b89bca8aa231614b4ba423620dd2e31993c75a9137-微信截图_20200517052852.png)

```javascript
const canFinish = (numCourses, prerequisites) => {
    const inDegree = new Array(numCourses).fill(0); // 入度数组
    const map = {};                                 // 邻接表
    for (let i = 0; i < prerequisites.length; i++) {
        inDegree[prerequisites[i][0]]++;              // 求课的初始入度值
        if (map[prerequisites[i][1]]) {               // 当前课已经存在于邻接表
            map[prerequisites[i][1]].push(prerequisites[i][0]); // 添加依赖它的后续课
        } else {                                      // 当前课不存在于邻接表
            map[prerequisites[i][1]] = [prerequisites[i][0]];
        }
    }
    // 6 
    // [[3, 0], [3, 1], [4, 1], [4, 2], [5, 3], [5, 4]]
    // console.log(inDegree) // [ 0, 0, 0, 2, 2, 2 ]
    // console.log(map) // { '0': [ 3 ], '1': [ 3, 4 ], '2': [ 4 ], '3': [ 5 ], '4': [ 5 ] }
    const queue = [];
    for (let i = 0; i < inDegree.length; i++) { // 所有入度为0的课入列
        if (inDegree[i] == 0) queue.push(i);
    }
    let count = 0;
    while (queue.length) {
        const selected = queue.shift();           // 当前选的课，出列
        count++;                                  // 选课数+1
        const toEnQueue = map[selected];          // 获取这门课对应的后续课
        if (toEnQueue && toEnQueue.length) {      // 确实有后续课
            for (let i = 0; i < toEnQueue.length; i++) {
                inDegree[toEnQueue[i]]--;             // 依赖它的后续课的入度-1
                if (inDegree[toEnQueue[i]] == 0) {    // 如果因此减为0，入列
                    queue.push(toEnQueue[i]);
                }
            }
        }
    }
    return count == numCourses; // 选了的课等于总课数，true，否则false
};
```

