# ARTS Week 14
>[![Bw84pt.jpg](https://s1.ax1x.com/2020/11/01/Bw84pt.jpg)](https://imgchr.com/i/Bw84pt)
>> 如果不竭尽全力到最后一刻的话，是无法取胜的 ——朝田诗乃 《刀剑神域2》

***
## Algoithm
>[分隔链表](https://leetcode-cn.com/problems/partition-list)

### 概述
给定一个链表和一个特定值 x，对链表进行分隔，使得所有小于 x 的节点都在大于或等于 x 的节点之前。

你应当保留两个分区中每个节点的初始相对位置。
 

示例:

输入: head = 1->4->3->2->5->2, x = 3
输出: 1->2->2->4->3->5

### 分析
将链表分割成 左右两个链表，然后依次进行遍历，这里需要注意的就是一定要注意，有可能会成为**环**！

### 代码

```python

# Definition for singly-linked list.
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None


class Solution:
    def partition(self, head: ListNode, x: int) -> ListNode:
        # 分成两个链表，>x 链表同步 <=x 链表失败
        # 左边链表 小于 x
        left = ListNode(-1)
        left_start = left

        # 右边链表 大于等于x
        right = ListNode(-1)
        right_start = right

        while head:
            # 因为 中间会产生 环，所以会及时同步
            node = head.next

            if head.val < x:
                left.next = head
                left = left.next
            else:
                right.next = head
                right = right.next

            # 这里是为了断开 环
            head.next = None
            head = node

        # 左右链表拼接
        left.next = right_start.next
        return left_start.next

```

### 总结
链表问题，可以通过多个指针进行解析，同时也需要注意防止成为 **环**

***
## Review
>[](todo)

### 概述
todo


***
## Tip
>[](todo)

### 概述
todo

***
## Share
>[](todo)

### 概述
todo