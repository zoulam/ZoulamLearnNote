# 链表操作

## 1、反转单链表

### 递归

```javascript
var reverseList = function (head) {
    if(head == null || head.next == null){
        return head
    }
    const current = reverseList(head.next);
    // (cur.next)
    // 例如，1，2，3，4，5,null current 是5 head是4回拨指针
    head.next.next = head;
    head.next = null
    return current;
};
```

### 迭代

```JavaScript
var reverseList = function (head) {
    let cur = head
    let prev = null
    while (cur) {
        let next = cur.next
        cur.next = prev
        prev = cur
        cur = next
    }
    return prev
};
```



## 2、 未排序链表去重

### 时间O(n2) 空间O(1)

```javascript
var removeDuplicateNodes = function (head) {
    let cur = head;
    while (cur) {
        let other = cur;
        while (other.next) {
            if (other.next.val == cur.val) {
                other.next = other.next.next;
            } else {
                other = other.next;
            }
        }
        cur = cur.next;
    }
    return head;
};
```

### 时间O(n) 空间O(n)

**用set辅助**

不重复两个指针都后移

重复，删除节点，prev不动，cur后移

```javascript
var removeDuplicateNodes = function (head) {
    if (!head) return head
    let cur = head.next,
        prev = head,
        set = new Set()

    set.add(head.val)
    while (cur) {
        if (set.has(cur.val)) {
            prev.next = cur.next
            cur = cur.next
        } else {
            set.add(cur.val)
            cur = cur.next
            prev = prev.next
        }
    }
    return head
};
```



## 3、 排序链表去重O（n）

```javascript
var deleteDuplicates = function (head) {
    if (!head) return head
    let prev = head
    let cur = head.next
    while (cur) {
        if (cur.val === prev.val) {
            prev.next = prev.next.next
            cur = prev.next
        } else {
            cur = cur.next
            prev = prev.next
        }
    }
    return head
};
```



## 4、 单链表删除节点

### 暴力

```JavaScript
var deleteNode = function (head, val) {
    if (head.val == val) return head.next
    let prev = head
    let curr = head.next
    while (curr) {
        if (curr.val == val) {
            prev.next = curr.next
        }
        prev = curr
        curr = curr.next
    }
    return head
};
```

### 递归

**删除就是剔除指针的引用关系**

```JavaScript
var deleteNode = function (head, val) {
    if(head.val == val){
        return head.next;
    }
    /**
     * 假设1，2，3，目标值是2
     * 当前head是1.
     * 本来head.next是2,但是调用deletenode函数的时候刚刚好2==2,把2（head）的下一个值3的指针返回回去
     * 所以head.next = 3
     * 1->3
     * 
    */
    head.next = deleteNode(head.next,val);
    // 节点1的next指向节点三
    return head;
};
```



## 5、 链表partition（分割）

> ​	一些不爽的点：
>
> ​			1、创建链表的节点每次都需要new一次，所以选择拷贝节点而不是拷贝值
>
> ​			2、拷贝的节点是一个链条，也就是还会带有别的值，这就是为什么p2后面还有2和1

**创建两个链表**，链表1存储小于目标数，链表2存储其他（即大于等于都可以）

如：

输入：[3,5,8,5,10,2,1]  5 

链表1：[-1,3,2,1] 链表2： [-1,5,8,5,10,2,1]  链表是链着的，p2是[10, 2, 1]

输出：[3,2,1,5,8,5,10]

**p是（pointer）**指针的缩写



```JavaScript
var partition = function (head, x) {
    if (!head) return head
    let node1 = new ListNode(-1)
    let node2 = new ListNode(-1)
    let p1 = node1
    let p2 = node2
    let curr = head
    while (curr) {
        if (curr.val < x) {
            p1.next = curr
            p1 = p1.next
        } else {
            p2.next = curr
            p2 = p2.next
        }
        curr = curr.next
    }

    if (!node1.next) {
        return head
    } else {
        p1.next = node2.next
        p2.next = null
        return node1.next
    }
};
```



## 6、 寻找链表倒数第K个节点

**思路是让快指针先走k步，然后同时出发，当快指针到达尾部，慢指针就在倒数k的位置**

```JavaScript
var getKthFromEnd = function (head, k) {
    let quick = head
    let slow = head
    for (let i = 1; i <= k; i++) {
        quick = quick.next
    }
    while (quick) {
        quick = quick.next
        slow = slow.next
    }
    return slow
};
```



## 7、 删除链表倒数第N个节点

**先找到他前面的一个节点， 再删除。**  

需要考虑n和链表等长的情况，（理解成删除头节点）

```JavaScript
var removeNthFromEnd = function (head, n) {
    let fast = head, slow = head
    // 快先走 n-1 步
    while(--n) {
        fast = fast.next
    }
    // 删除头节点的情况
    if(!fast.next) return head.next
    fast = fast.next
    // fast、slow 一起前进
    while(fast && fast.next) {
        fast = fast.next
        slow = slow.next
    }
    slow.next = slow.next.next
    return head
};
```

### 更好理解的方式

**不用考虑删除头节点时的越界问题**

```JavaScript
var removeNthFromEnd = function(head, n) {
    let preHead = new ListNode(0)
    preHead.next = head
    let fast = preHead, slow = preHead
    // 快先走 n 步
    while(n--) {
        fast = fast.next
    }
    // fast、slow 一起前进
    while(fast && fast.next) {
        fast = fast.next
        slow = slow.next
    }
    slow.next = slow.next.next
    return preHead.next
};
```



## 8、 判断链表是否为回文链表

### 空间O(n) 时间O(n)

```javascript
var isPalindrome = function (head) {
    let arr = []
    while (head) {
        arr.push(head.val)
        head = head.next
    }

    for (let i = 0, j = arr.length - 1; i <= j; i++ , j--) {
        if (arr[i] !== arr[j]) return false
    }
    return true
};
```

### 空间O(1) 时间O(n)

```javascript
var isPalindrome = function (head) {
    if (head == null || head.next == null) {
        return true;
    }
    let fast = head;
    let slow = head;
    let prev;
    // 链表 1 2 3 3 2 1
    // prev停在第一个三 [3,3,2,1] slow停在第二个3 fast指向null(辅助指针)
    while (fast && fast.next) {
        prev = slow;
        slow = slow.next;
        fast = fast.next.next;
    }
    prev.next = null;  // 断成两个链表
    // 翻转后半段
    let head2 = null;
    while (slow) {
        const tmp = slow.next;
        slow.next = head2;
        head2 = slow;
        slow = tmp;
    }
    // 比对
    while (head && head2) {
        if (head.val != head2.val) {
            return false;
        }
        head = head.next;
        head2 = head2.next;
    }
    return true;
};
```



## 9、 判断链表是否有环

```JavaScript
    let slow = head
    let fast = head
    while (fast && fast.next) {
        slow = slow.next
        fast = fast.next.next
        if (fast == slow) return true
    }
    return false
```



## 10、 环形链表第一个入环节点

<img src="https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/链表环的入口.jpg" alt="链表环的入口" style="zoom: 50%;" />

```javascript
var detectCycle = function(head) {
    if(!head || !head.next) return null
    let slow = head
    let fast = head
    while(fast != null && fast.next != null) {
        slow = slow.next
        fast = fast.next.next
        if(slow === fast) { // 有环
            fast = head // 将D覆盖为A
            while(slow != fast) {
                slow = slow.next
                fast = fast.next
            }
            return slow
        }
    }
    return null
};
```



## 11、 两个链表的第一个公共节点

### 时间O(n) 空间O(1)

```javascript
var getIntersectionNode = function (headA, headB) {
    let node1 = headA,
        node2 = headB
    while (node1 != node2) {
        // 到尾部就会重置回去，极大可能不会在第一轮相遇
        // 如果没有公共节点，将会在最小公倍数的地方指向null 中断while
        node1 = node1 !== null ? node1.next : headB
        node2 = node2 !== null ? node2.next : headA
    }
    return node1
};
```

