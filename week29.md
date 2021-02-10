# ARTS Week 29

> ![](https://github.com/Carmenliukang/ARTS/blob/master/image/29/1.jpeg)
>> 他死在一个谁都不知道角落我不想他的骨灰毫无意义。

***

## Algoithm

> [递增顺序查找树](https://leetcode-cn.com/problems/increasing-order-search-tree/)

### 概述

给你一个树，请你 按中序遍历 重新排列树，使树中最左边的结点现在是树的根，并且每个结点没有左子结点，只有一个右子结点。

示例 ：

    输入：[5,3,6,2,4,null,8,1,null,null,null,7,9]
    
           5
          / \
        3    6
       / \    \
      2   4    8
     /        / \ 
    1        7   9
    
    输出：[1,null,2,null,3,null,4,null,5,null,6,null,7,null,8,null,9]
    
     1
      \
       2
        \
         3
          \
           4
            \
             5
              \
               6
                \
                 7
                  \
                   8
                    \
                     9  

提示：

给定树中的结点数介于 1 和 100 之间。

每个结点都有一个从 0 到 1000 范围内的唯一整数值。

### 分析
可以使用两种方式处理：
1. 中序遍历+生成树
2. 中序遍历+节点同步

### coding

```python


# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
    
class Solution:
    def __init__(self):
        self.res = TreeNode()
        self.ans = self.res

    def increasingBST(self, root: TreeNode) -> TreeNode:
        self.dfs(root)
        return self.res.right
    
    def dfs(self,root):
        if root is None:
            return 
        
        self.dfs(root.left)
        root.left = None
        self.ans.right = root
        self.ans = root
        self.dfs(root.right)
        

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

> [todo](todo)

### 概述

todo