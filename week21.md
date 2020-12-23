# ARTS Week 20

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

> [todo](todo)
>

### 概述

todo