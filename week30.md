# ARTS Week 30

> ![](https://github.com/Carmenliukang/ARTS/blob/master/image/30/1.jpeg)
>> 人生没有彩排，每一天都是现场直播。

***

## Algoithm

> [二叉树中的最大路径和](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)

### 概述

路径 被定义为一条从树中任意节点出发，沿父节点-子节点连接，达到任意节点的序列。同一个节点在一条路径序列中 至多出现一次 。该路径 至少包含一个 节点，且不一定经过根节点。

路径和 是路径中各节点值的总和。

给你一个二叉树的根节点 root ，返回其 最大路径和 。

示例 1：

![](https://github.com/Carmenliukang/ARTS/blob/master/image/30/2.jpg)

    输入：root = [1,2,3]
    输出：6
    解释：最优路径是 2 -> 1 -> 3 ，路径和为 2 + 1 + 3 = 6

示例 2：

![](https://github.com/Carmenliukang/ARTS/blob/master/image/30/3.jpg)

    输入：root = [-10,9,20,null,null,15,7]
    输出：42
    解释：最优路径是 15 -> 20 -> 7 ，路径和为 15 + 20 + 7 = 42

提示：

    树中节点数目范围是 [1, 3 * 104]
    -1000 <= Node.val <= 1000

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
        self.res = float("-inf")

    def maxPathSum(self, root: TreeNode) -> int:
        if not root:
            return 0
        self.dfs(root)
        return self.res

    def dfs(self, root):
        if not root:
            return 0
        left = max(self.dfs(root.left), 0)
        right = max(self.dfs(root.right), 0)
        total = root.val + left + right

        self.res = max(self.res, total, root.val)
        return root.val + max(left, right)
```

***

## Review

> [Redis-消息队列](https://time.geekbang.org/column/article/284291)

### 概述

消息队列:
1. 消息保存
2. 处理重复消息
3. 保证消息可靠性

### 基于 List 的消息队列解决方案
1. List 本身就是按先进先出的顺序对数据进行存取的，所以，如果使用 List 作为消息队列保存消息的话，就已经能满足消息保序的需求了。

非阻塞

![](https://github.com/Carmenliukang/ARTS/blob/master/image/30/7.jpg)


阻塞

![](https://github.com/Carmenliukang/ARTS/blob/master/image/30/8.jpg)

### 基于 Streams 的消息队列解决方案

Streams 是 Redis 专门为消息队列设计的数据类型，它提供了丰富的消息队列操作命令。XADD：插入消息，保证有序，可以自动生成全局唯一 ID；XREAD：用于读取消息，可以按 ID 读取数据；XREADGROUP：按消费组形式读取消息；XPENDING 和 XACK：XPENDING 命令可以用来查询每个消费组内所有消费者已读取但尚未确认的消息，而 XACK 命令用于向消息队列确认消息处理已完成。

***

## Tip

> [Redis-保存时间序列数据](https://time.geekbang.org/column/article/282478)

### 概述

时间序列：以时间为关键字段，进行聚合分析

方案列表：

1. Hash
2. Hash + Sorted Set
3. RedisTimeSeries

### Hash

1. 单数据查询适合
2. 范围查询较差
   ![](https://github.com/Carmenliukang/ARTS/blob/master/image/30/4.jpg)

### Hash + Sorted Set

1. 单数据查询适合
2. 范围查询也适合
   
![](https://github.com/Carmenliukang/ARTS/blob/master/image/30/4.jpg)

使用原子性命令保证其原子性操作

![](https://github.com/Carmenliukang/ARTS/blob/master/image/30/6.jpg)

### RedisTimeSeries

RedisTimeSeries 是 Redis 的一个扩展模块。它专门面向时间序列数据提供了数据类型和访问接口，并且支持在 Redis 实例上直接对数据进行按时间范围的聚合计算。

因为 RedisTimeSeries 不属于 Redis 的内建功能模块，在使用时，我们需要先把它的源码单独编译成动态链接库 redistimeseries.so，再使用 loadmodule 命令进行加载，如下所示：

```
loadmodule redistimeseries.so
```

1. 用 TS.CREATE 命令创建时间序列数据集合；
2. 用 TS.ADD 命令插入数据；
3. 用 TS.GET 命令读取最新数据；
4. 用 TS.MGET 命令按标签过滤查询数据集合；
5. 用 TS.RANGE 支持聚合计算的范围查询。

***

## Share

> [不同路径题目总结](https://github.com/Carmenliukang/ARTS/blob/master/week30.md#share)

### 概述

[不同路径](https://leetcode-cn.com/problems/unique-paths)

[不同路径 II](https://leetcode-cn.com/problems/unique-paths-ii)

### 总结

```python

class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        # 生成相关的 二维数组
        dp = [[0] * n for _ in range(m)]
        # 第一行特殊逻辑处理
        for i in range(m):
            dp[i][0] = 1

        # 第一列特殊逻辑处理
        for j in range(n):
            dp[0][j] = 1

        for i in range(1, m):
            for j in range(1, n):
                # 考虑变量，一次进行递归
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
        return dp[-1][-1]
```

