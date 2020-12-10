# ARTS Week 20
>![DzELm6.md.jpg](https://s3.ax1x.com/2020/12/07/DzELm6.md.jpg)
>> 你沒能拯救你母親是因為你沒有力量，而我沒有與巨人對抗是因為我沒有勇氣。 --《进击的巨人》

***
## Algoithm
>[最长同值路径](https://leetcode-cn.com/problems/longest-univalue-path/)

### 概述
给定一个二叉树，找到最长的路径，这个路径中的每个节点具有相同值。 这条路径可以经过也可以不经过根节点。

注意：两个节点之间的路径长度由它们之间的边数表示。

    示例 1:
    
    输入:
    
                  5
                 / \
                4   5
               / \   \
              1   1   5
    输出:
    
    2
    示例 2:
    
    输入:
    
                  1
                 / \
                4   5
               / \   \
              4   4   5
    输出:


注意: 给定的二叉树不超过10000个结点。 树的高度不超过1000。


### 分析
>[Chuancey 解答](https://leetcode-cn.com/problems/longest-univalue-path/solution/guan-yu-di-gui-si-lu-de-chao-xiang-xi-ge-ren-jian-/)

递归的几个核心问题：
1. 输入参数
2. 输出结果
3. 函数功能
4. 终止条件

方法论：

1. 将二叉树看成 左子树/根节点/右子树 
2. 左/右子树最长同值路径

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
        self.max_num = 0

    def longestUnivaluePath(self, root: TreeNode) -> int:
        self.dfs(root)
        return self.max_num
    
    def dfs(self,root):
        if not root:
            return 0
            
        left_length = self.dfs(root.left)
        right_length = self.dfs(root.right)
        
        left_arrow = right_arrow = 0
        # 如果分别计算左右子树的同步
        if root.left and root.val == root.left.val:
            left_arrow = left_length + 1
        
        if root.right and root.val == root.right.val:
            right_arrow = right_length + 1

        self.max_num = max(self.max_num,left_arrow+right_arrow)

        return max(left_arrow,right_arrow)
```

***
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