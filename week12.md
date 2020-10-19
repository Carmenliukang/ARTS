# ARTS Week 12
>![](https://image.uc.cn/s/wemedia/s/upload/2020/c3aad88339c5233577627dfb7844e0a8.png)
>> 坚强不是结果，是朝某个目标努力的过程！


***
## Algoithm
> [奇偶链表](https://leetcode-cn.com/problems/odd-even-linked-list)

### 概述
给定一个单链表，把所有的奇数节点和偶数节点分别排在一起。请注意，这里的奇数节点和偶数节点指的是节点编号的奇偶性，而不是节点的值的奇偶性。

请尝试使用原地算法完成。你的算法的空间复杂度应为 O(1)，时间复杂度应为 O(nodes)，nodes 为节点总数。

示例 1:

输入: 1->2->3->4->5->NULL
输出: 1->3->5->2->4->NULL
示例 2:

输入: 2->1->3->5->6->4->7->NULL
输出: 2->3->6->7->1->5->4->NULL
说明:

应当保持奇数节点和偶数节点的相对顺序。
链表的第一个节点视为奇数节点，第二个节点视为偶数节点，以此类推。


### 分析
1. 通过位置，而不是通过里面的数值判断。
2. 画出来
![](https://pic.leetcode-cn.com/00bd1d974b5a2e6d7d4faf0d5baad1c691f4ed8963cb1b7133d1112bad4c5e86-image.png)
### 代码

```python

# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next


class Solution:
    def oddEvenList(self, head: ListNode) -> ListNode:
        if not head:
            return head
        # 使用指针进行同步，注意，这里是通过位置判断是否为奇偶，而不是通过判断里面的位置来进行判断。
        odd = head
        even = head.next
        even_start = even

        while even and even.next:
            odd.next = even.next
            odd = odd.next

            even.next = odd.next
            even = even.next

        odd.next = even_start
        return head

```


### 总结
链表的问题都可以通过画图的形式展示出来，然后一步一步运行。


***
## Review
> [TODO](TODO)

### 概述
TODO

***
## Tip
TODO
> [redis RDB](https://time.geekbang.org/column/article/271839)

### 概述
TODO

***
## Share
TODO
>[linux fork](https://zhuanlan.zhihu.com/p/36872365)

### 概述
TODO

