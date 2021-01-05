# ARTS Week 23

> [![r79Xwj.jpg](https://s3.ax1x.com/2020/12/28/r79Xwj.jpg)](https://imgchr.com/i/r79Xwj)
>> 如果不竭尽全力到最后一刻的话，是无法取胜的。 --《刀剑神域》

***

## Algoithm

> [前序遍历构造二叉搜索树](https://leetcode-cn.com/problems/construct-binary-search-tree-from-preorder-traversal)

### 概述

返回与给定前序遍历preorder 相匹配的二叉搜索树（binary search tree）的根结点。

(回想一下，二叉搜索树是二叉树的一种，其每个节点都满足以下规则，对于node.left的任何后代，值总 < node.val，而 node.right 的任何后代，值总 > node.val。此外，前序遍历首先显示节点node
的值，然后遍历 node.left，接着遍历 node.right。）

题目保证，对于给定的测试用例，总能找到满足要求的二叉搜索树。

    示例：
    
    输入：[8,5,1,7,10,12]
    输出：[8,5,10,1,7,null,12]

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/03/08/1266.png)

提示：

1. 1 <= preorder.length <= 100
2. 1 <= preorder[i]<= 10^8
3. preorder 中的值互不相同

### 分析

使用二叉搜索树的特性，依次进行递归生成

### codeing

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:
    def bstFromPreorder(self, preorder: list[int]) -> TreeNode:
        self.ids = 0
        self.preorder = preorder
        return self.dfs(float("-inf"), float("inf"))

    def dfs(self, low, upper):
        if self.ids == len(self.preorder):
            return None

        val = self.preorder[self.ids]
        if val < low or val > upper:
            return None

        root = TreeNode(val)
        self.ids += 1
        root.left = self.dfs(low, val)
        root.right = self.dfs(val, upper)
        return root

```

### 总结

1. 忽略左右子树，将其看成一个单点去思考设计代码。

***

## Review

> [todo](todo)

### 概述

todo

***

## Tip

> [todo](todo)

### 概述

todo

***

## Share

> [思考未来同步加油](https://github.com/Carmenliukang/ARTS/blob/master/week23.md#share)

### 概述

todo