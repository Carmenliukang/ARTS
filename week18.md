# ARTS Week 
>[![DryJYV.jpg](https://s3.ax1x.com/2020/11/27/DryJYV.jpg)](https://imgchr.com/i/DryJYV)
>> 他死在一个谁都不知道角落我不想他的骨灰毫无意义。

***
## Algoithm
>[找树左下角的值](https://leetcode-cn.com/problems/find-bottom-left-tree-value)

### 概述
    给定一个二叉树，在树的最后一行找到最左边的值。
    
    示例 1:
    
    输入:
    
        2
       / \
      1   3
    
    输出:
    1
     
    
    示例 2:
    
    输入:
    
            1
           / \
          2   3
         /   / \
        4   5   6
           /
          7
    
    输出:
    7
 

注意: 您可以假设树（即给定的根节点）不为 NULL。

### 分析
这里是可以通过前序遍历，记录每一层的最左边节点的数据，然后同步结果。


### codeing

```python

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None


class Solution:
    def __init__(self):
        self.res = []

    def findBottomLeftValue(self, root: TreeNode) -> int:
        self.dfs(root, 0)
        return None if not self.res else self.res[-1]

    def dfs(self, node, depth):
        # 使用前序 记录每一层的数据
        if not node:
            return

        if depth == len(self.res):
            self.res.append(node.val)

        depth += 1
        self.dfs(node.left, depth)
        self.dfs(node.right, depth)

    def findBottomLeftValueMethod(self, root: TreeNode) -> int:
        # 使用栈的方式同步其每一层的数据，同时记录其相关的结果
        queue = [root]
        while queue:
            node = queue.pop(0)
            if node.right:  # 先右后左
                queue.append(node.right)
            if node.left:
                queue.append(node.left)
        return node.val

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