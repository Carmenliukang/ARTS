# ARTS Week 
>[![DUMpnK.jpg](https://s3.ax1x.com/2020/11/25/DUMpnK.jpg)](https://imgchr.com/i/DUMpnK)
>> 仰望星空

***
## Algoithm
>[二叉树展开为链表](https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list)

### 概述

    给定一个二叉树，原地将它展开为一个单链表。

    例如，给定二叉树
    
        1
       / \
      2   5
     / \   \
    3   4   6
    将其展开为：
    
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


### 分析

1. 先使用前序遍历，获取每一个指针的数据
2. 将每一个节点的数据同步

### coding

```python

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:

    def flatten(self, root: TreeNode) -> None:
        """
        Do not return anything, modify root in-place instead.
        """
        if not root:
            return
        # 先获取每一个节点的指针
        nodes = []

        # 这里使用的是前序遍历
        def dfs(root):
            if not root:
                return None

            nodes.append(root)
            dfs(root.left)
            dfs(root.right)

        dfs(root)

        # 这里使用的是同步生成相关节点，前序遍历会自动生成其相关的节点。
        for i in range(1, len(nodes)):
            pre, cur = nodes[i - 1], nodes[i]
            pre.left = None
            pre.right = cur

```

***
## Review
>[When DRY Doesn’t Work, Go WET](https://medium.com/better-programming/when-dry-doesnt-work-go-wet-6befda0444bf)

### 概述
介绍了DRY 原则


***
## Tip
>[架构分解：边界，不断重新审视边界](https://time.geekbang.org/column/article/170912)

### 概述
1. 架构就是业务的正交分解。每个模块都有它自己的业务。
2. 不断重新审视边界


***
## Share
>[N叉树构建](https://github.com/Carmenliukang/ARTS/blob/master/week16.md#share)


### 概述
```python

class Node(object):
    def __init__(self, val=None):
        self.val = val
        self.children = []  # 这里必须是一个有序，所以选择的是数组


class Tree(object):
    # 创建N叉树
    def create(self, val, data):
        # 递归调用创建N叉树
        root = Node(val=val)
        for i in data.get(val, []):
            node = self.create(val=i, data=data)
            root.children.append(node)
        return root

    def show(self, root):
        for i in root.children:
            self.show(i)


if __name__ == '__main__':
    node = []
    data = {
        1: [2, 3],
        2: [6, 7],
        7: [5]
    }

    tree = Tree()
    root = tree.create(1, data)

    tree.show(root)

```  