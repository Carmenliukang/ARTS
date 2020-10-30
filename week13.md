# ARTS Week 13
>[![BliudS.jpg](https://s1.ax1x.com/2020/10/27/BliudS.jpg)](https://imgchr.com/i/BliudS)
>> 未來的旅程是如此遙遠，能見到的光明卻是如此稀少。即使如此——我仍然沒有完全放棄希望。


***
## Algoithm
>[链表中的下一个更大节点](https://leetcode-cn.com/problems/next-greater-node-in-linked-list)


### 概述
给出一个以头节点 head 作为第一个节点的链表。链表中的节点分别编号为：node_1, node_2, node_3, ... 。

每个节点都可能有下一个更大值（next larger value）：对于 node_i，如果其 next_larger(node_i) 是 node_j.val，那么就有 j > i 且  node_j.val > node_i.val，而 j 是可能的选项中最小的那个。如果不存在这样的 j，那么下一个更大值为 0 。

返回整数答案数组 answer，其中 answer[i] = next_larger(node_{i+1}) 。

注意：在下面的示例中，诸如 [2,1,5] 这样的输入（不是输出）是链表的序列化表示，其头节点的值为 2，第二个节点值为 1，第三个节点值为 5 。


示例 1：
    
    输入：[2,1,5]
    输出：[5,5,0]

示例 2：
    
    输入：[2,7,4,3,5]
    输出：[7,0,5,5,0]
    
示例 3：
    
    输入：[1,7,5,1,9,2,5,1]
    输出：[7,9,9,9,0,5,0,0]

### 分析
这里可以使用 栈的方式同步


### 分析

```python

# Definition for singly-linked list.
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None


class Solution:
    def nextLargerNodes(self, head: ListNode) -> list[int]:
        # 转为顺序表
        cur = head
        nums = []
        while cur:
            nums.append(cur.val)
            cur = cur.next

        # 求解
        stack = []
        # 最后结果的 list
        result = [0 for i in range(len(nums))]
        # 倒序 读取 list
        # 开始从 list 倒数第二个 读取，是因为最后一个一定是 0
        for j in range(len(result) - 1, -1, -1):

            if not stack:
                stack.append(nums[j])
            else:
                x = nums[j]
                while stack and stack[-1] <= x:
                    stack.pop()

                # 因为这里已经默认设置成0了，所以不用修改。
                if stack:
                    result[j] = stack[-1]
                stack.append(x)

        return result

```


### 总结
对于 范围大小的问题，可以通过使用栈的方式同步。

***
## Review
>![](https://miro.medium.com/max/700/1*mSE6ialQ-9TcGUz5ts_FMA.png)
> [An Efficient Git Branching Strategy Every Developer Should Know](https://medium.com/better-programming/efficient-git-branching-strategy-every-developer-should-know-f1034b1ba041)

### 概述
常规的开发流程

![](https://miro.medium.com/max/700/1*eZMKRu5BrdTSRhr-JqwyVw.jpeg)


1. You can have feature testing from the QA branch and regression from a stable develop branch post-merging of all the features that are planned in the current release.
2. develop would always be stable and any developer can cut their feature branch at any point in time.
3. You won’t be spamming the commit history of the develop branch.
4. If QA faces issues with any feature branch, then you might fix it and push without reverting if that feature is independent of others.
5. Hotfix: In case of any prod issue, cut a branch from master, fix it, and release.

![](https://miro.medium.com/max/700/1*XV7ACDj6_GSCRh1nFSa73A.jpeg)
***
## Tip
> [MySQL join 图解](http://wxb.github.io/2016/12/15/MySQL%E4%B8%AD%E7%9A%84%E5%90%84%E7%A7%8Djoin.html)
> [mysql index leng limit](https://stackoverflow.com/questions/15157227/mysql-varchar-index-length)


### 概述

#### Mysql join
Mysql join 图片

[![B1Z52F.jpg](https://s1.ax1x.com/2020/10/28/B1Z52F.jpg)](https://imgchr.com/i/B1Z52F)


#### MySQL index limit
解释了MySQL index 为什么有 str 255 leng 的长度限制，这里可以对 json/text 格式进行 255 长度的字段索引，提高效率


***
## Share
> [leetcode>linked-list](https://leetcode-cn.com/tag/linked-list/)

### 概述
最近在做链表相关的题目，同时也总结反思了一些常见的一些算法的框架

链表常用的几种方式：
1. 遍历链表所有数值存储到数组中，然后进行计算。
2. 指针：双指针、快慢指针、多指针 等
3. 递归


#### 方法一
遍历链表所有数值，存储到数组中，然后进行遍历

>[从未到头遍历链表](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof)

输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。

 

示例 1：

输入：head = [1,3,2]
输出：[2,3,1]
 

限制：

0 <= 链表长度 <= 10000

**解析**：因为这里需要使用的是将链表的数据全部打印出来，可以先将链表 val 全部获取后，再进行翻转

```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None


class Solution:
    def reversePrint(self, head):
        res = []
        while head:
            # res.insert(0,head.val) # 这种方式，因为需要 频繁的进行 IO 的切换，所以占用很多的 CPU 等。
            res.append(head.val)  # 这种方式就是后面追加，io方面的操作会减少很多
            head = head.next  # 这里是作为一个调用方式的原理
        return res[::-1]  # 进行最后结果的一个倒序

```

##### 思考
因为 数组每次的最后的插入都会导致 重新申请内存空间，所以选择将链表数据全部写到 数组中，然后再进行翻转


#### 方法二
使用指针的方式同步计算

>[环路检测](https://leetcode-cn.com/problems/linked-list-cycle-lcci/solution/rang-ni-zhong-wen-shu-xue-tui-li-kuai-man-zhi-zhen/)

给定一个链表，如果它是有环链表，实现一个算法返回环路的开头节点。
有环链表的定义：在链表中某个节点的next元素指向在它前面出现过的节点，则表明该链表存在环路。

 

示例 1：

输入：head = [3,2,0,-4], pos = 1
输出：tail connects to node index 1
解释：链表中有一个环，其尾部连接到第二个节点。
 

示例 2：

输入：head = [1,2], pos = 0
输出：tail connects to node index 0
解释：链表中有一个环，其尾部连接到第一个节点。
 

示例 3：

输入：head = [1], pos = -1
输出：no cycle
解释：链表中没有环。

```python

# Definition for singly-linked list.
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None


class Solution:
    def detectCycle(self, head: ListNode) -> ListNode:
        fast = head
        slow = head

        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next

            if fast == slow:
                while head != fast:
                    head = head.next
                    fast = fast.next
                return head

```


##### 思考
快慢指针方式，能够快速的同步状态

#### 方法三
递归方式解决问题

给定两个用链表表示的整数，每个节点包含一个数位。

这些数位是反向存放的，也就是个位排在链表首部。

编写函数对这两个整数求和，并用链表形式返回结果。

 

示例：

输入：(7 -> 1 -> 6) + (5 -> 9 -> 2)，即617 + 295
输出：2 -> 1 -> 9，即912
进阶：思考一下，假设这些数位是正向存放的，又该如何解决呢?

示例：

输入：(6 -> 1 -> 7) + (2 -> 9 -> 5)，即617 + 295
输出：9 -> 1 -> 2，即912

```python

# Definition for singly-linked list.
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None


class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        """
        使用非递归的方式计算
        :param l1: 链表1
        :param l2: 链表
        :return: 返回链表
        """
        root = ListNode(-1)
        tmp = root
        carry = 0

        # 边界条件判断，不仅仅是 链表是否为空，同时也需要判断 是否有进位。
        while l1 or l2 or carry > 0:
            # 判断节点是否为空
            l1_val = l1.val if l1 else 0
            l2_val = l2.val if l2 else 0

            # 节点数值求和
            val = l1_val + l2_val + carry

            # 计算进位
            carry = val // 10
            # 生成节点，因为是 最小位在前，所以需要取余。
            node = ListNode(val if val < 10 else val % 10)
            tmp.next = node

            # 判断是否有下一步节点
            l1 = l1.next if l1 and l1.next else 0
            l2 = l2.next if l2 and l2.next else 0

            tmp = tmp.next

        return root.next

    def addTwoNumbersMethod1(self, l1: ListNode, l2: ListNode) -> ListNode:
        """
        使用递归的方式进行同步，递归的方式 按照以下的流程：
        1. 确定终止条件
        2. 确定每次循环条件
        3. 完成递归循环

        :param l1: 链表
        :param l2: 链表
        :return: 链表
        """
        root = ListNode(-1)
        tmp = root
        self.helper(tmp, l1, l2, 0)
        return root.next

    def helper(self, tmp, l1, l2, currt):
        if not l1 and not l2 and currt == 0:
            return
        l1_val = l1.val if l1 else 0
        l2_val = l2.val if l2 else 0
        total = l1_val + l2_val + currt
        currt = total // 10

        node = ListNode(total % 10)
        tmp.next = node

        l1 = l1.next if l1 and l1.next else 0
        l2 = l2.next if l2 and l2.next else 0

        return self.helper(tmp.next, l1, l2, currt)


```

### 常用方法总结
这部分用于总结相关的账户问题

#### 生成链表
```python

# Definition for singly-linked list.
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None

class Solution:
    def createListNode(self,data):
        root = ListNode(-1)
        head = root
        for i in data:
            node = ListNode(i)
            head.next = node
            head = head.next
        return root.next

```

#### 遍历链表

```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None


class Solution:
    def Print(self, head):
        res = []
        while head:
            res.append(head.val)
            head = head.next
        return res

```

#### 翻转链表

```python

class ListNode(object):
    def __init__(self, data):
        self.data = data
        self.next = None


class Reverse(object):
    '翻转链表'

    def reverseList(self, head: ListNode):
        """
        这里遍历方法，将其每一个数据进行同步
        :param head: LNode 实例
        :return: LNode or None
        """

        # 排除其特殊情况
        if head is None or head.next is None:
            return head

        pre = None  # 开始位置
        tmp = None  # 下一个元素
        cur = None  # next 的位置

        tmp = head

        while tmp:
            # 将相关的指针指针进行翻转
            cur = tmp.next
            tmp.next = pre

            pre = tmp
            tmp = cur

        return pre


```

#### 快慢指针
```python

# Definition for singly-linked list.
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None


class Solution:
    def detectCycle(self, head: ListNode) -> ListNode:
        """
        快慢指针的方式同步状态
        """
        fast = head
        slow = head

        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next

            if fast == slow:
                while head != fast:
                    head = head.next
                    fast = fast.next
                return head
```

#### 递归方法
>[链表求和](https://leetcode-cn.com/problems/sum-lists-lcci)

输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。

 

示例 1：

输入：head = [1,3,2]
输出：[2,3,1]
 

限制：

0 <= 链表长度 <= 10000

```python

# Definition for singly-linked list.
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None


class Solution:
    def addTwoNumbersMethod1(self, l1: ListNode, l2: ListNode) -> ListNode:
        """
        使用递归的方式进行同步，递归的方式 按照以下的流程：
        1. 确定终止条件
        2. 确定每次循环条件
        3. 完成递归循环

        :param l1: 链表
        :param l2: 链表
        :return: 链表
        """
        root = ListNode(-1)
        tmp = root
        self.helper(tmp, l1, l2, 0)
        return root.next

    def helper(self, tmp, l1, l2, currt):
        if not l1 and not l2 and currt == 0:
            return
        l1_val = l1.val if l1 else 0
        l2_val = l2.val if l2 else 0
        total = l1_val + l2_val + currt
        currt = total // 10

        node = ListNode(total % 10)
        tmp.next = node

        l1 = l1.next if l1 and l1.next else 0
        l2 = l2.next if l2 and l2.next else 0

        return self.helper(tmp.next, l1, l2, currt)

```

