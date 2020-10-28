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
> [TODO](TODO)

### 概述
TODO


***
## Tip
> [MySQL join 图解](http://wxb.github.io/2016/12/15/MySQL%E4%B8%AD%E7%9A%84%E5%90%84%E7%A7%8Djoin.html)


### 概述
Mysql join 图片

[![B1Z52F.jpg](https://s1.ax1x.com/2020/10/28/B1Z52F.jpg)](https://imgchr.com/i/B1Z52F)


***
## Share
TODO
> [TODO](TODO)

### 概述
TODO
