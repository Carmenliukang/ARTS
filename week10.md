# ARTS Week 9
>![](https://s1.ax1x.com/2020/10/10/0sf7z8.jpg)
>> 拜托，请把力量借给软弱的我，给我从这里再度起身迈步的力量。


***
## Algoithm
> [两数相加 ||](https://leetcode-cn.com/problems/add-two-numbers-ii/)

### 概述
给你两个 非空 链表来代表两个非负整数。数字最高位位于链表开始位置。它们的每个节点只存储一位数字。将这两数相加会返回一个新的链表。

你可以假设除了数字 0 之外，这两个数字都不会以零开头。


进阶：

如果输入链表不能修改该如何处理？换句话说，你不能对列表中的节点进行翻转。

 

示例：

输入：(7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 8 -> 0 -> 7


### 分析
1. 先遍历获取其val，然后对数值进行处理。，这种是比较菜的一种方式，其实应该可以使用 栈的方式同步
2. 这里也可以使用 栈的方式进行同步，先进后出 方式计算最后的结果
### 代码

```python

# Definition for singly-linked list.
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None


class Solution:

    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        """
        1. 特殊情况判断
        2. 使用 list 先进后出的方式，进行最后结果的计算
        """
        p1, p2 = l1, l2
        nums1, nums2 = self.get_val(p1), self.get_val(p2)
        place, total = 0, 0
        while nums1 or nums2:
            num1 = nums1.pop() if nums1 else 0
            num2 = nums2.pop() if nums2 else 0
            total += (num1 + num2) * pow(10, place)
            place += 1

        total_list = list(str(total))
        return self.create_list(total_list)

    def addTwoNumbersMehtod(self, l1: ListNode, l2: ListNode) -> ListNode:
        """
        1. 特殊情况处理
        2. 遍历链表，list
        3. 计算链表的最终结果，这里需要 使用 pow() 这个内置的方法，这样系统会好很多。
        """

        if not l1 or not l2:
            return l1 or l2

        # 链表 val 获取，翻转，因为高位在前
        p1, p2 = l1, l2
        l1_list, l2_list = self.get_val(p1), self.get_val(p2)
        l1_list = l1_list[::-1]
        l2_list = l2_list[::-1]

        # 计算这里的结果同步
        total = 0
        l1_len = len(l1_list)
        l2_len = len(l2_list)
        for i in range(max(l1_len, l2_len)):
            l1_num = l1_list[i] if i < l1_len else 0
            l2_num = l2_list[i] if i < l2_len else 0
            total += (l1_num + l2_num) * (pow(10, i))

        total_list = list(str(total))

        return self.create_list(total_list)

    def get_val(self, l):
        # 获取每一个结果的数值
        res = []
        while l:
            res.append(l.val)
            l = l.next

        return res

    def create_list(self, nodes):
        # 创建一个链表
        root = ListNode(0)
        p = root
        for i in nodes:
            node = ListNode(i)
            p.next = node
            p = p.next

        return root.next

```


### 总结
链表基本操作
对于相关的计算可以使用栈的方式进行快速的同步和计算

```Python

class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None

def get_val(self, l):
        # 获取每一个结果的数值
        res = []
        while l:
            res.append(l.val)
            l = l.next

        return res
   

def create_list(self, nodes):
        # 创建一个链表
        root = ListNode(0)
        p = root
        for i in nodes:
            node = ListNode(i)
            p.next = node
            p = p.next

        return root.next
```


***
## Review
> [The Must-Know Clean Code Principles](https://medium.com/swlh/the-must-know-clean-code-principles-1371a14a2e75)

### 概述
描述了一些常用的代码简洁的常用规则


***
## Tip
> [redis RDB](https://time.geekbang.org/column/article/271839)

### 概述
1. 内存快照 方式存储

2. COW 方式实现修改的时候依然可以全量获取数据 

    操作系统提供的写时复制技术（Copy-On-Write, COW）


![](https://s1.ax1x.com/2020/10/10/0yEcpn.jpg)

***
## Share
>[linux fork](https://zhuanlan.zhihu.com/p/36872365)

### 概述

fork的实现分为以下两步
1. 复制进程资源
2. 执行该进程

复制进程的资源包括以下几步
1. 进程pcb
2. 程序体，即代码段数据段等
3. 用户栈
4. 内核栈
5. 虚拟内存池
6. 页表

![](https://pic3.zhimg.com/80/v2-c5c3ba7e5e1f3eb0127683faef9cce32_720w.jpg)



