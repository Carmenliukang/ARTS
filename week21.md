# ARTS Week 21

> [![ryON1H.md.jpg](https://s3.ax1x.com/2020/12/23/ryON1H.md.jpg)](https://imgchr.com/i/ryON1H)
>> 人生就像一杯茶，不会苦一辈子，但总会苦一阵子。

***

## Algoithm

> [从前序和后序遍历构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-postorder-traversal)

### 概述

返回与给定的前序和后序遍历匹配的任何二叉树。

pre 和 post 遍历中的值是不同的正整数。

    示例：
    
    输入：pre = [1,2,4,5,3,6,7], post = [4,5,2,6,7,3,1]
    输出：[1,2,3,4,5,6,7]

### 分析

* 前序遍历：
    1. 根节点 左子树 右子树
* 后序遍历
    1. 左子树 右子树 根节点

例如，如果最终的二叉树可以被序列化的表述为 [1, 2, 3, 4, 5, 6, 7]，那么其前序遍历为 [1] + [2, 4, 5] + [3, 6, 7]，而后序遍历为 [4, 5, 2] + [6, 7, 3] + [1].

如果我们知道左分支有多少个结点，我们就可以对这些数组进行分组，并用递归生成树的每个分支。

我们令左分支有 L个节点。我们知道左分支的头节点为 pre[1]，但它也出现在左分支的后序表示的最后。所以 pre[1] = post[L-1]（因为结点的值具有唯一性），因此 L = post.indexOf(pre[1]) + 1。

现在在我们的递归步骤中，左分支由 pre[1 : L+1] 和 post[0 : L] 重新分支，而右分支将由 pre[L+1 : N] 和 post[L : N-1] 重新分支。

### code

```python

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None


class Solution:
    def constructFromPrePost(self, pre: list[int], post: list[int]) -> TreeNode:
        return self.dfs(pre, post)

    def dfs(self, pre, post):
        if not pre:
            return None
        root = TreeNode(pre[0])

        if len(pre) == 1:
            return root

        # 因为这个是唯一的数值，所以这里才能够确定去唯一数值
        L = post.index(pre[1]) + 1
        root.left = self.dfs(pre[1:L + 1], post[:L])
        root.right = self.dfs(pre[L + 1:], post[L:])
        return root

```

#### 总结

> [三种遍历方式生成二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-postorder-traversal/solution/kan-wo-jiu-gou-liao-san-chong-bian-li-fang-shi-gou/
)


***

## Review

> [Software developers might be obsolete by 2030](https://towardsdatascience.com/software-developers-might-be-obsolete-by-2030-cb5ddbfec291)

### 概述

文章简单分析了现状：
1. AI 自动化方面已经有很大的提升
2. 不久的将来可能会替换程序员工作

***

## Tip

> [软件质量管理：单元测试、持续构建与发布](https://time.geekbang.org/column/article/188797)

### 概述
1. 项目管理：
[![r5KZ2n.png](https://s3.ax1x.com/2020/12/27/r5KZ2n.png)](https://imgchr.com/i/r5KZ2n)
2. 项目开发周期：
[![r5K154.png](https://s3.ax1x.com/2020/12/27/r5K154.png)](https://imgchr.com/i/r5K154)
3. 方向管理：
  1. 单元测试
  2. 持续构建，持续发布

***

## Share

>[树算法的类型题目总结](https://github.com/Carmenliukang/ARTS/blob/master/week21.md#share)

### 概述
递归的方法论：
1. 输入参数
2. 输出参数
3. 函数体：对于不同情况进行分类处理
4. 递归顺序

#### 二叉树的最大 类型题目

#### [二叉树的最大路径和](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum)

给定一个非空二叉树，返回其最大路径和。

本题中，路径被定义为一条从树中任意节点出发，沿父节点-子节点连接，达到任意节点的序列。该路径至少包含一个节点，且不一定经过根节点。



示例 1：
  
  输入：[1,2,3]
  
         1
        / \
       2   3
  
  输出：6  

示例 2：

  输入：[-10,9,20,null,null,15,7]
  
     -10
     / \
    9  20
      /  \
     15   7
  
  输出：42

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


#### [二叉树的最长的连续序列](https://leetcode-cn.com/problems/binary-tree-longest-consecutive-sequence-ii)

给定一个二叉树，你需要找出二叉树中最长的连续序列路径的长度。

请注意，该路径可以是递增的或者是递减。例如，[1,2,3,4] 和 [4,3,2,1] 都被认为是合法的，而路径 [1,2,4,3] 则不合法。另一方面，路径可以是 子-父-子 顺序，并不一定是 父-子 顺序。

示例 1:

输入:

          1
         / \
        2   3

输出: 2

解释: 最长的连续路径是 [1, 2] 或者 [2, 1]。


示例 2:

输入:

      2
     / \
    1   3

输出: 3

解释: 最长的连续路径是 [1, 2, 3] 或者 [3, 2, 1]。


注意: 树上所有节点的值都在 [-1e7, 1e7] 范围内。


```python



# Definition for a binary tree node.
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None


class Solution:
    def longestConsecutive(self, root: TreeNode) -> int:
        # todo  这里还是需要再次进行深入的反馈。
        # 根节点必定处于左子树的最长连续递增序列或右子树最长连续递减序列中 或者 同时处于二者之中
        # 处于左子树的最长连续递减序列或右子树的最长连续递增序列中 或者 同时处于二者之中
        # 处于仅由自身构成的递增、递减序列中
        # 只可能处于以上三种情况中
        # 故包含根节点的最长连续序列路径的长度 = 包含根节点的最长连续递增序列的长度 +
        # 包含根节点的最长连续递减序列的长度 - 1（根节点被重复算了一次）
        # 每个子树的根节点都含有两种状态：第一是包含该根节点的最长连续递增序列的长度
        # 第二是包含该根节点的最长连续递减序列的长度
        ans = 0

        def dfs(root: TreeNode) -> int:
            if not root: return [0, 0]
            nonlocal ans
            # status[0] 包含根节点的最长递增子序列的长度
            # status[1] 包含根节点的最长递减子序列的长度
            # 均初始化为 1
            status = [1] * 2

            left_status = dfs(root.left)
            right_status = dfs(root.right)

            if root.left:
                # 判断左子树处于连续递增序列中还是处于连续递减序列中
                if root.left.val + 1 == root.val:  # 处于连续递增序列中
                    status[0] = left_status[0] + 1
                elif root.left.val - 1 == root.val:  # 处于连续递减序列中
                    status[1] = left_status[1] + 1

            if root.right:
                # 判断右子树处于连续递增序列中还是处于连续递减序列中
                if root.right.val + 1 == root.val:  # 处于连续递增序列中
                    status[0] = max(right_status[0] + 1, status[0])
                elif root.right.val - 1 == root.val:  # 处于连续递减序列中
                    status[1] = max(right_status[1] + 1, status[1])

            # 最长连续序列可能处于左子树或右子树中，而不经过当前根节点
            # 故需要将中间结果进行比较
            ans = max(ans, status[0] + status[1] - 1)
            return status

        dfs(root)
        return ans


class SolutionMethod:
    def __init__(self):
        self.res = 0

    def longestConsecutive(self, root: TreeNode) -> int:
        self.dfs(root)
        return self.res

    def dfs(self, root):
        if not root:
            # 这里也是需要返回结果。因为根节点为0
            return 0, 0

        inc, dcr = 1, 1

        left_inc, left_dcr = self.dfs(root.left)
        right_inc, right_dcr = self.dfs(root.right)

        if root.left:
            if root.val == root.left.val + 1:
                inc = left_inc + 1
            if root.val == root.left.val - 1:
                dcr = left_dcr + 1

        if root.right:
            if root.val == root.right.val + 1:
                inc = max(right_inc + 1, inc)
            if root.val == root.right.val - 1:
                dcr = max(right_dcr + 1, dcr)
        # 计算以此节点为根节点的数据统计。
        self.res = max(self.res, inc + dcr - 1)
        return inc, dcr

```