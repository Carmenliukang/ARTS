# ARTS Week 15
>[![BhNHNF.jpg](https://s1.ax1x.com/2020/11/06/BhNHNF.jpg)](https://imgchr.com/i/BhNHNF)
>> 不论是我或是其它任何人，都不是为了死亡而存在，是为了活下去而活着。 ——《刀剑神域》

***
## Algoithm
>[平衡二叉树||](https://leetcode-cn.com/problems/ping-heng-er-cha-shu-lcof)

### 概述
输入一棵二叉树的根节点，判断该树是不是平衡二叉树。如果某二叉树中任意节点的左右子树的深度相差不超过1，那么它就是一棵平衡二叉树。

 

示例 1:

给定二叉树 [3,9,20,null,null,15,7]

        3
       / \
      9  20
        /  \
       15   7
返回 true 。

示例 2:

给定二叉树 [1,2,2,3,3,null,null,4,4]

           1
          / \
         2   2
        / \
       3   3
      / \
     4   4
返回 false 。

 

限制：

1 <= 树的结点个数 <= 10000


### 解析
树结构的题目都可以通过

前序、中序、后序 遍历同步解决相关的问题。

这里的方法分为两种：
1. 中序遍历+剪纸
2. 前序遍历 这里会有重复计算


### code
```python


# Definition for a binary tree node.
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None


class Solution:
    def isBalanced(self, root: TreeNode) -> bool:
        # 计算器深度，判断最后的结果，通过判断最后的机器同步
        def sequence(node):
            if not node:
                return 0
            left = sequence(node.left)
            if left == -1:
                return -1

            right = sequence(node.right)
            if right == -1:
                return -1
            # 如果左右子树，高度相差为1，那么其深度就是 左右子树的最大数值
            return max(left, right) + 1 if abs(left - right) <= 1 else -1

        return sequence(root) != -1

    def isBalancedMethod(self, root: TreeNode) -> bool:
        # 这里使用的是前序遍历，这种会造成 重复计算 ，所以这里的问题还是需要同步的
        if not root:
            return True
        return abs(self.depth(root.left) - self.depth(root.right)) <= 1 and self.isBalanced(
            root.left) and self.isBalanced(root.right)

    def depth(self, node):
        if not node:
            return 0
        left = self.depth(node.left)
        right = self.depth(node.right)
        return max(left, right) + 1
```



***
## Review
>[![DSrnLF.png](https://s3.ax1x.com/2020/11/13/DSrnLF.png)](https://imgchr.com/i/DSrnLF)
>[Don’t Use Database Generated IDs in Domain Entitie](https://medium.com/swlh/dont-use-database-generated-ids-d703d35e9cc4)

### 概述
**Generate your IDs at the application level. Not at the persistence level.** 

(please note that entity ID is not necessarily a database primary key)


example:
[![DSraee.png](https://s3.ax1x.com/2020/11/13/DSraee.png)](https://imgchr.com/i/DSraee)

example1：
[![DSrBFA.png](https://s3.ax1x.com/2020/11/13/DSrBFA.png)](https://imgchr.com/i/DSrBFA)

optimize:
[![DSrcy8.png](https://s3.ax1x.com/2020/11/13/DSrcy8.png)](https://imgchr.com/i/DSrcy8)


link:
[When and where to determine the ID of en entity by Matthias Noback](https://matthiasnoback.nl/2018/05/when-and-where-to-determine-the-id-of-an-entity/)



***
## Tip
>[心性：架构师的修炼之道](https://time.geekbang.org/column/article/166014)

### 概述
**架构修炼之道**

心性上:
1. 认同他人能力
2. 保持好奇心和韧性
3. 学会否定自己

技能:
1. 理解需求的能力
2. 读代码的能力
3. 抽象系统的能力


***
## Share
>[celert 中文手册](https://www.celerycn.io/)

### 概述
**celery 中文手册**

celery 是依靠 Redis、RabbitMQ 作为中间件 的分布式 消息队列工具

Celery 通过消息机制进行通信，通常使用中间人（Broker）作为客户端和职程（Worker）调节。启动一个任务，客户端向消息队列发送一条消息，然后中间人（Broker）将消息传递给一个职程（Worker），最后由职程（Worker）进行执行中间人（Broker）分配的任务。





 