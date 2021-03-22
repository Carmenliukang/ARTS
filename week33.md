# ARTS Week 33

> ![](https://github.com/Carmenliukang/ARTS/blob/master/image/33/1.jpg)
>> 天将降大任于斯人也，必先苦其心志，劳其筋骨，饿其体肤，空乏其身，行拂乱其所为也，所以动心忍性，增益其所不能。

***

## Algoithm

> [二维区域和检索 - 矩阵不可变](https://leetcode-cn.com/problems/range-sum-query-2d-immutable)

### 概述

给定一个二维矩阵，计算其子矩形范围内元素的总和，该子矩阵的左上角为 (row1, col1) ，右下角为 (row2, col2) 。

![](https://github.com/Carmenliukang/ARTS/blob/master/image/33/2.png)

上图子矩阵左上角 (row1, col1) = (2, 1) ，右下角(row2, col2) = (4, 3)，该子矩形内元素的总和为 8。

    示例：
    
    给定 matrix = [
      [3, 0, 1, 4, 2],
      [5, 6, 3, 2, 1],
      [1, 2, 0, 1, 5],
      [4, 1, 0, 1, 7],
      [1, 0, 3, 0, 5]
    ]

    sumRegion(2, 1, 4, 3) -> 8
    sumRegion(1, 1, 2, 2) -> 11
    sumRegion(1, 2, 2, 4) -> 12

提示：

1. 你可以假设矩阵不可变。
2. 会多次调用 sumRegion 方法。
3. 你可以假设 row1 ≤ row2 且 col1 ≤ col2 。

#### 分析

这里需要使用的是 前缀和 方式同步修改计算

#### coding

```python

class NumMatrix:

    def __init__(self, matrix: list[list[int]]):
        self.martix = matrix
        self._total_martix()

    def _total_martix(self):
        m, n = len(self.martix), len(self.martix[0])
        # 这里加一的原因  是 list 起始位置都为0。及 0,0 一定是0
        dp = [[0] * (n + 1) for _ in range(m + 1)]

        for i in range(m):
            for j in range(n):
                dp[i + 1][j + 1] = dp[i][j + 1] + dp[i + 1][j] - dp[i][j] + self.martix[i][j]
        self.dp = dp

    def sumRegion(self, row1: int, col1: int, row2: int, col2: int) -> int:
        total = self.dp[row2 + 1][col2 + 1] - self.dp[row2 + 1][col1] - self.dp[row1][col2 + 1] + self.dp[row1][col1]
        return total

# # Your NumMatrix object will be instantiated and called as such:
# # obj = NumMatrix(matrix)
# # param_1 = obj.sumRegion(row1,col1,row2,col2)


```

***

## Review

> [5 Signs That You’re Wasting Your Life As A Developer](https://medium.com/madhash/5-signs-that-youre-wasting-your-life-as-a-developer-131607ff1998)

### 概述

Once you get a job, it’s easy to get stuck into trading your time for a paycheck. You take the commute, get into the
office, brew your coffee, take it to your desk, sit down, and start tapping.

Sometimes, there might be a meeting here and there, a debate with your fellow developers over a module or
implementation.

You end your day by going home, going to sleep, and then waking up to do it all over again.

It sounds monotonous. But you convinced yourself otherwise.

Because work is part of life — just like bills, rent, utilities, groceries, and every other little thing in life that
chips away at your paycheck.

Then one day, you stare at your reflection and asking life’s ultimate question: is this it?

Here are 5 signs that you’re wasting your life as a developer, the symptoms and how to remedy it.

1. You’ve forgotten your dreams

    1. Solution: write down all your dreams and pick your top 5

“The comfort zone is a psychological state in which one feels familiar, safe, at ease, and secure. You never change your
life until you step out of your comfort zone; change begins at the end of your comfort zone.” ― Roy T. Bennett

2. Your projects are boring
    1. Your projects are boring


3. You can’t see your future you

4. Your edge of knowledge is still the same
5. You dread your day
    1. Solution: Take a break and reassess your life

***

## Tip

> [Redis-变慢原因](https://time.geekbang.org/column/article/287819)

### 概述

1. 文件系统
2. 操作系统

![](https://github.com/Carmenliukang/ARTS/blob/master/image/33/3.jpg)

#### 文件系统：AOF 模式

AOF日志支持 ：

1. no
2. everysec
3. always

文件系统策略：

1. write
    1. 只写入内河缓存区，然后返回。不用等待日志写入到磁盘中。
2. fsync
    1. 日志写入磁盘后返回，耗时比较长

![](https://github.com/Carmenliukang/ARTS/blob/master/image/33/4.jpg)

**细节点：**

1. everysec
    1. 可以允许丢失1s的操作
    2. 后台子线程异步调用 fsync

2. always
    1. 主进程同步写入
    2. 每次操作都会写入磁盘。保证不会丢失。

AOF 重写机制

1. 避免AOF日志过大
2. 使用子进程同步
3. 潜在问题：
    1. AOF 重写会对磁盘进行大量 IO 操作，同时，fsync 又需要等到数据写到磁盘后才能返回，所以，当 AOF 重写的压力比较大时，就会导致 fsync 被阻塞。虽然 fsync 是由后台子线程负责执行的，但是，主线程会监控
       fsync 的执行进度。
    2. 当主线程使用后台子线程执行了一次 fsync，需要再次把新接收的操作记录写回磁盘时，如果主线程发现上一次的 fsync 还没有执行完，那么它就会阻塞。所以，如果后台子线程执行的 fsync 频繁阻塞的话（比如 AOF
       重写占用了大量的磁盘 IO 带宽），主线程也会阻塞，导致 Redis 性能变慢。

![](https://github.com/Carmenliukang/ARTS/blob/master/image/33/5.jpg)

##### 解决方案

1. 查看配置 appendfsync
2. 如果业务应用对延迟非常敏感，但同时允许一定量的数据丢失，那么，可以把配置项 no-appendfsync-on-rewrite 设置为 yes，
3. 使用固态硬盘

#### 操作系统：swap

内存 swap 是操作系统里将内存数据在内存和磁盘间来回换入和换出的机制，涉及到磁盘的读写，所以，一旦触发 swap，无论是被换入数据的进程，还是被换出数据的进程，其性能都会受到慢速磁盘读写的影响。

原因：物理内存不足

1. Redis 实例自身使用了大量的内存，导致物理机器的可用内存不足；
2. 和 Redis 实例在同一台机器上运行的其他进程，在进行大量的文件读写操作。文件读写本身会占用系统内存，这会导致分配给 Redis 实例的内存量变少，进而触发 Redis 发生 swap。

##### 解决方案

1. redis-cli info | grep process_id
2. cd /proc/5332
3. cat smaps | egrep '^(Swap|Size)'

```shell

$cat smaps | egrep '^(Swap|Size)'
Size: 584 kB
Swap: 0 kB
Size: 4 kB
Swap: 4 kB
Size: 4 kB
Swap: 0 kB
Size: 462044 kB
Swap: 462008 kB
Size: 21392 kB
Swap: 0 kB
```

每一行 Size 表示的是 Redis 实例所用的一块内存大小，而 Size 下方的 Swap 和它相对应，表示这块 Size 大小的内存区域有多少已经被换出到磁盘上了。如果这两个值相等，就表示这块内存区域已经完全被换出到磁盘了。

作为内存数据库，Redis 本身会使用很多大小不一的内存块，所以，你可以看到有很多 Size 行，有的很小，就是 4KB，而有的很大，例如 462044KB。不同内存块被换出到磁盘上的大小也不一样，例如刚刚的结果中的第一个 4KB
内存块，它下方的 Swap 也是 4KB，这表示这个内存块已经被换出了；另外，462044KB 这个内存块也被换出了 462008KB，差不多有 462MB。

***

## Share

> [0-1背包类型题目总结](https://github.com/Carmenliukang/ARTS/blob/master/week33.md#share)

### 概述

背包问题（Knapsack problem）是一种组合优化的NP完全（NP-Complete，NPC）问题。

问题通用描述：

给定一组物品，每种物品都有自己的重量和价格，在限定的总重量内，我们如何选择，才能使得物品的总价格最高

#### 0/1背包问题

##### 问题

最基本的背包问题就是01背包问题（01 knapsack problem）：

一共有N件物品，第i（i从1开始）件物品的重量为w[i]，价值为v[i]。 在总重量不超过背包承载上限W的情况下，能够装入背包的最大价值是多少？

##### 0/1 背包问题

状态 N个物品 W重量

选择 第I个物品 是否放入 F[i][w] = max(F[i-1][w],(F[i-1][w-Wt[i]]+val[i]))

考虑边界条件 w-Wt[i]>0

##### code

```python
def knapsack_0_1(W, N, wt, val):
    """
    0-1 背包问题总结
    :param W: 背包最大可承受重量
    :param N: 背包最大可接收数量
    :param wt: list[int] 每个物品的重量
    :param val: list[int] 每个物品的价值
    :return: int 最大价值的数量
    """
    dp = [[0] * (W + 1) for _ in range(N + 1)]

    for i in range(N):
        for j in range(W):
            # 这里有边界问题，因为可能小于0的情况
            if j - wt[i - 1] < 0:
                dp[i][j] = dp[i - 1][j]
            else:
                dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - wt[i - 1]] + val[i - 1])

    return dp[N][W]

```

#### 0/1 完全背包

一共有N种物品，每种数量不限，第i（i从1开始）件物品的重量为w[i]，价值为v[i]。 在总重量不超过背包承载上限W的情况下，能够装入背包的最大价值是多少？

核心转换方程： dp[i][j] = max(dp[i - 1][j], dp[i][j - wt[i - 1]] + val[i - 1])

