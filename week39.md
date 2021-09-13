# ARTS Week 39

> ![](https://github.com/Carmenliukang/ARTS/blob/master/image/39/kristaps-ungurs-bqRUmr144Rc-unsplash.jpg)
>> 凡事过往，皆为序章

***

## Algoithm

> [从字符串生成二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-string)

### 概述

你需要从一个包括括号和整数的字符串构建一棵二叉树。

输入的字符串代表一棵二叉树。它包括整数和随后的 0 ，1 或 2 对括号。整数代表根的值，一对括号内表示同样结构的子树。

若存在左子结点，则从左子结点开始构建。

示例：

    输入："4(2(3)(1))(6(5))"
    输出：返回代表下列二叉树的根节点:
    
           4
         /   \
        2     6
       / \   /
      3   1 5

提示：

输入字符串中只包含'(', ')', '-'和'0' ~ '9'

空树由""而非"()"表示。

### 分析

二叉树的题目，总体按照先序/中序/后序 方式进行递归生成。

1. 首先判断终止条件：
    1. 如果为 "" ，则为 root
    2. 如果没有 "("，那么说明完整组成 root节点

2. 进行左右子树分割
    1. 按照格"()" 进行切割
    2. 如果组合为0，且开始节点保持一致，那么就说明是左子树，否则为右子树。

3. 递归生成

### code

```python


# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:
    def str2tree(self, s: str) -> TreeNode:
        root = self.dfs(s)
        return root

    def dfs(self, s):
        if len(s) == 0:
            return None
        if '(' not in s:
            return TreeNode(int(s[:]))
        pos = s.index('(')
        root = TreeNode(int(s[0: pos]))
        start = pos  # （start是某棵子树，区间的起始index，第一个左括号）
        cnt = 0
        for i in range(pos, len(s)):
            if s[i] == '(':
                cnt += 1
            elif s[i] == ')':
                cnt -= 1

            if cnt == 0 and start == pos:  # 左子的部分
                root.left = self.str2tree(s[start + 1: i])
                start = i + 1
            elif cnt == 0 and start != pos:  # 右子的部分 这个地方必须用 elif!!!!!!
                root.right = self.str2tree(s[start + 1: i])

        return root

```

***

## Review

> [todo](todo)

### 概述

todo

***

## Tip

> [推荐阅读：分布式系统架构经典资料](https://time.geekbang.org/column/article/2080)

### 概述

分布式框架的相关资料汇总

### 基础概念

* CAP 定理
* Fallacies of Distributed Computing

### 经典资料

* Distributed systems theory for the distributed systems engineer
* FLP Impossibility Result
* An introduction to distributed systems
* Distributed Systems for fun and profit
* Distributed Systems: Principles and Paradigms
* Scalable Web Architecture and Distributed Systems
* Principles of Distributed Systems
* Making reliable distributed systems in the presence of software errors
* Designing Data Intensive Applications

### CAP 定理

CAP 定理是分布式系统设计中最基础，也是最为关键的理论。它指出，分布式数据存储不可能同时满足以下三个条件。

* 一致性（Consistency）：每次读取要么获得最近写入的数据，要么获得一个错误。
* 可用性（Availability）：每次请求都能获得一个（非错误）响应，但不保证返回的是最新写入的数据。
* 分区容忍（Partition tolerance）：尽管任意数量的消息被节点间的网络丢失（或延迟），系统仍继续运行。

![](https://github.com/Carmenliukang/ARTS/blob/master/image/39/1.webp)


***

## Share

> [todo](todo)

### 概述

todo