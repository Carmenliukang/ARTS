# ARTS Week 30

> ![](https://github.com/Carmenliukang/ARTS/blob/master/image/30/1.jpeg)
>> 人生没有彩排，每一天都是现场直播。

***

## Algoithm

> [二叉树中的最大路径和](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)

### 概述

路径 被定义为一条从树中任意节点出发，沿父节点-子节点连接，达到任意节点的序列。同一个节点在一条路径序列中 至多出现一次 。该路径 至少包含一个 节点，且不一定经过根节点。

路径和 是路径中各节点值的总和。

给你一个二叉树的根节点 root ，返回其 最大路径和 。

示例 1：

![](https://github.com/Carmenliukang/ARTS/blob/master/image/30/2.jpg)

    输入：root = [1,2,3]
    输出：6
    解释：最优路径是 2 -> 1 -> 3 ，路径和为 2 + 1 + 3 = 6

示例 2：

![](https://github.com/Carmenliukang/ARTS/blob/master/image/30/3.jpg)

    输入：root = [-10,9,20,null,null,15,7]
    输出：42
    解释：最优路径是 15 -> 20 -> 7 ，路径和为 15 + 20 + 7 = 42

提示：

    树中节点数目范围是 [1, 3 * 104]
    -1000 <= Node.val <= 1000

### code

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:
    def __init__(self):
        self.res = float("-inf")

    def maxPathSum(self, root: TreeNode) -> int:
        if not root:
            return 0
        self.dfs(root)
        return self.res

    def dfs(self, root):
        if not root:
            return 0
        left = max(self.dfs(root.left), 0)
        right = max(self.dfs(root.right), 0)
        total = root.val + left + right

        self.res = max(self.res, total, root.val)
        return root.val + max(left, right)
```

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

> [不同路径题目总结](https://github.com/Carmenliukang/ARTS/blob/master/week30.md#share)

### 概述

[不同路径](https://leetcode-cn.com/problems/unique-paths)

[不同路径 II](https://leetcode-cn.com/problems/unique-paths-ii)

### 总结

```python

class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        # 生成相关的 二维数组
        dp = [[0] * n for _ in range(m)]
        # 第一行特殊逻辑处理
        for i in range(m):
            dp[i][0] = 1

        # 第一列特殊逻辑处理
        for j in range(n):
            dp[0][j] = 1

        for i in range(1, m):
            for j in range(1, n):
                # 考虑变量，一次进行递归
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
        return dp[-1][-1]
```

