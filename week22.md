# ARTS Week 22

> [![r50wcV.jpg](https://s3.ax1x.com/2020/12/27/r50wcV.jpg)](https://imgchr.com/i/r50wcV)
>> 如果不自己主动去接触的话，是什么也无法创造的。 --优纪《刀剑神域》

***

## Algoithm

> [二叉搜索树序列](https://leetcode-cn.com/problems/bst-sequences-lcci)

### 概述

从左向右遍历一个数组，通过不断将其中的元素插入树中可以逐步地生成一棵二叉搜索树。给定一个由不同节点组成的二叉搜索树，输出所有可能生成此树的数组。

示例： 给定如下二叉树

        2
       / \
      1   3

返回：

    [
       [2,1,3],
       [2,3,1]
    ]

#### 分析

> [引用题解](https://leetcode-cn.com/problems/bst-sequences-lcci/solution/15xing-dai-ma-by-suibianfahui/)

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

> [How to build your own Neural Network from scratch in Python](https://towardsdatascience.com/how-to-build-your-own-neural-network-from-scratch-in-python-68998a08e4f6)

### 概述

#### What’s a Neural Network?

Most introductory texts to Neural Networks brings up brain analogies when describing them. Without delving into brain
analogies, I find it easier to simply describe Neural Networks as a mathematical function that maps a given input to a
desired output.

Neural Networks consist of the following components

* An input layer, x
* An arbitrary amount of hidden layers
* An output layer, ŷ
* A set of weights and biases between each layer, W and b
* A choice of activation function for each hidden layer, σ. In this tutorial, we’ll use a Sigmoid activation function.

![](https://miro.medium.com/max/500/1*sX6T0Y4aa3ARh7IBS_sdqw.png)

#### Training the Neural Network

![](https://miro.medium.com/max/355/1*E1_l8PGamc2xTNS87XGNcA.png)


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

> [时刻保证竞争力](https://github.com/Carmenliukang/ARTS/blob/master/week22.md#share)

### 概述

现在的技术更新速度愈发加快，同时因为疫情，现在的大环境降低，导致内卷愈演愈烈，同时寡头效应初现，所以还是需要让自己有足够的积累，以及在某一方面 有非常深刻的理解。否则，随着工作年限的提升，很难赶上现在对工作年限要求的能力的提升速度。
那么应该如何应对：

1. 坚持刷算法题目：
    1. 二叉树，二叉树，二叉树 一定要先将二叉树简单/中等 尽可能的全部刷完
    2. 类型题目，能够将部分中等题目刷一定比例。
    3. 输出方法论

2. 专注某一个技术：
    1. 不论技术如何变化，但是底层的技术都是 IO模型的选择和优化，所以需要对某一个中间件/数据库 能够有非常全面的深入的理解。
    2. 设计模式的应用，每一个底层逻辑都会使用不同的设计模型，不仅仅要知道，同时也需要进行思考：
        1. 为什么选择这种模式？
        2. 是否可以选择其他的模式？
        3. 如果让你设计，你会如何设计？

3. 提升自我能力：
    1. 提升协调资源的能力，能够将事情做好很难，所以如果将一件事情能够做好，才是最基础的。
    2. 提升系统能力