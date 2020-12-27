# ARTS Week 20

> [![r50wcV.jpg](https://s3.ax1x.com/2020/12/27/r50wcV.jpg)](https://imgchr.com/i/r50wcV)
>> 如果不自己主动去接触的话，是什么也无法创造的。 --优纪《刀剑神域》

***

## Algoithm

> [二叉搜索树序列](https://leetcode-cn.com/problems/bst-sequences-lcci)

### 概述
从左向右遍历一个数组，通过不断将其中的元素插入树中可以逐步地生成一棵二叉搜索树。给定一个由不同节点组成的二叉搜索树，输出所有可能生成此树的数组。

示例：
给定如下二叉树

        2
       / \
      1   3

返回：
    
    [
       [2,1,3],
       [2,3,1]
    ]

#### 分析
>[引用题解](https://leetcode-cn.com/problems/bst-sequences-lcci/solution/15xing-dai-ma-by-suibianfahui/)

* 使用一个queue存储下个所有可能的节点
* 然后选择其中一个作为path的下一个元素
* 递归直到queue元素为空
* 将对应的path加入结果中
* 由于二叉搜索树没有重复元素, 而且每次递归的使用元素的顺序都不一样, 所以自动做到了去重


#### code
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

    def BSTSequences(self, root: TreeNode) -> list[list[int]]:
        if not root:
            return [[]]
        self.dfs(root, [], [root.val])
        return self.res

    def dfs(self, root, q, path):
        # todo 这里可以类比多少种 BFS树等
        if not root:
            return

        if root.left:
            q.append(root.left)
        if root.right:
            q.append(root.right)

        if not q:
            self.res.append(path)

        for i, nex in enumerate(q):
            new_path = q[:i] + q[i + 1:]
            self.dfs(nex, new_path, path + [nex.val])


```

***

## Review

> [todo](todo)

### 概述

todo

***

## Tip

> [开源、云服务与外包管理](https://time.geekbang.org/column/article/190127)
> [软件版本迭代的规划](https://time.geekbang.org/column/article/191679)

### 概述
1. 开源是一种非常优秀的方式，如果可以希望能够参加一些开源项目，不仅能够提升自我技术，同时也能够在管理项目方面有更加高的提升。
2. 外部服务的顺序：云服务>开源>传统外包
3. 版本迭代方面：不同的时间段，还是需要进行不同的方向的优化。比如 golang 不同版本的迭代。

***

## Share

> [todo](todo)

### 概述

todo