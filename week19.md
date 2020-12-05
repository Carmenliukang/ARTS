# ARTS Week 
>[![DoUaXq.jpg](https://s3.ax1x.com/2020/12/02/DoUaXq.jpg)](https://imgchr.com/i/DoUaXq)
>> todo

***
## Algoithm
>[首个共同的祖先](https://leetcode-cn.com/problems/first-common-ancestor-lcci)

### 概述
设计并实现一个算法，找出二叉树中某两个节点的第一个共同祖先。不得将其他的节点存储在另外的数据结构中。注意：这不一定是二叉搜索树。

    例如，给定如下二叉树: root = [3,5,1,6,2,0,8,null,null,7,4]
    
        3
       / \
      5   1
     / \ / \
    6  2 0  8
      / \
     7   4
    示例 1:
    
    输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
    输出: 3
    解释: 节点 5 和节点 1 的最近公共祖先是节点 3。
    示例 2:
    
    输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
    输出: 5
    解释: 节点 5 和节点 4 的最近公共祖先是节点 5。因为根据定义最近公共祖先节点可以为节点本身。
    说明:
    
    所有节点的值都是唯一的。
    p、q 为不同节点且均存在于给定的二叉树中。

### 思考
1. 每一个节点都是唯一
2. p&q 一定都在树中

分成三种情况：
1. p/q 分别在左右子树中，那么最近的节点就是 root节点
2. p&q 都在同一子树中，那么就返回其子树的 root节点
3. p/q 其中一个为最近的节点，那么直接返回其节点

然后依次进行遍历
***

### coding

```python

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None


class Solution:
    """
    1. 题目中说明 节点都是 唯一
    2. p & q 都在树中

    按照不同情况区分：
    1. p/q 在root的左右子树中
    2. p&q 在root的同一个子树中
    3. p/q 为其中一个跟节点

    依次按照这种情况进行递归查询

    """
    def lowestCommonAncestor(self, root: TreeNode, p: TreeNode, q: TreeNode) -> TreeNode:
        if not root or root == p or root == q:
            return root

        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)
        if not left:
            return right

        if not right:
            return left

        if left and right:
            return root

```

### 总结
1. 树的问题大部分都可以通过递归来进行解决

递归的方法论：
1. 终止条件：
    1. not root
    2. left==right 
    3. root==p
2. 递归问题：
    1. 将问题进行分解，分成不同的情况进行沟通处理。
3. 递归顺序：
    1. 前序
    2. 中序
    3. 后序

## Review
>[todo]()

### 概述
todo 


***
## Tip
>[todo]()

### 概述
todo


***
## Share
>[todo]()

### 概述
todo  