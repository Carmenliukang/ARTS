# ARTS Week 34

> ![](https://github.com/Carmenliukang/ARTS/blob/master/image/34/1.jpg)
>> 不胜，则亡。但，不战，就不会胜利。

***

## Algoithm

> [零钱兑换](https://leetcode-cn.com/problems/coin-change/)

### 概述

给定不同面额的硬币 coins 和一个总金额 amount。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回-1。

你可以认为每种硬币的数量是无限的。

示例1：

输入：coins = [1, 2, 5], amount = 11 输出：3 解释：11 = 5 + 5 + 1 示例 2：

输入：coins = [2], amount = 3 输出：-1 示例 3：

输入：coins = [1], amount = 0 输出：0 示例 4：

输入：coins = [1], amount = 1 输出：1 示例 5：

输入：coins = [1], amount = 2 输出：2

提示：

1 <= coins.length <= 12 1 <= coins[i] <= 231 - 1 0 <= amount <= 104

#### coding

```python

class Solution:
    def coinChange(self, coins: list[int], amount: int) -> int:
        dp = [float("inf")] * (amount + 1)

        dp[0] = 0

        for i in range(1, amount + 1):
            for coin in coins:
                if i >= coin:
                    dp[i] = min(dp[i], dp[i - coin] + 1)

        return dp[amount] if dp[amount] != float("inf") else -1


```

***

## Review

> [12 Things I Stole From People More Successful Than Me](https://medium.com/mind-cafe/12-things-i-stole-from-people-more-successful-than-me-d0ef5ef6cd84)

### 概述

1. Pare Down The Number of Decisions You Make Every Day
2. Tear Up Your To-Do List
3. Turn “Have-To” Into “Get-To”
4. Use People’s Favorite Sound
5. Look At People’s Feet
6. Mise En Place
7. Don’t Be A Donkey
8. Stop Using The Number 7
9. Be A Whiner
10. Take Sabbaticals From Your Work
11. Never Ask For Someone’s ‘Opinion’
12. Practice What’s It’s Like To Be Poor

***

## Tip

> [Redis-删除数据后内存未减少](https://time.geekbang.org/column/article/289140)

### 概述

redis 删除后，还没有释放内存。

1. 当数据删除后，Redis 释放的内存空间会由内存分配器管理，并不会立即返回给操作系统。所以，操作系统仍然会记录着给 Redis 分配了大量内存。

### 内存碎片

类比车厢中的数据
![](https://github.com/Carmenliukang/ARTS/blob/master/image/34/2.jpg)

#### 原因

内因是操作系统的内存分配机制，外因是 Redis 的负载特征。

#### 内因：内存分配器的分配策略

Redis 可以使用 libc、jemalloc、tcmalloc 多种内存分配器来分配内存，默认使用 jemalloc。

jemalloc 的分配策略之一，是按照一系列固定的大小划分内存空间，例如 8 字节、16 字节、32 字节、48 字节，…, 2KB、4KB、8KB 等。当程序申请的内存最接近某个固定值时，jemalloc 会给它分配相应大小的空间。

#### 外因：键值对大小不一样和删改操作

1. 存储数据的大小不一致

![](https://github.com/Carmenliukang/ARTS/blob/master/image/34/3.jpg)

2. 键值对会被修改和删除,导致空间的扩容和释放

![](https://github.com/Carmenliukang/ARTS/blob/master/image/34/4.jpg)

#### 查看方法 INFO 命令

```shell

INFO memory
# Memory
used_memory:1073741736
used_memory_human:1024.00M
used_memory_rss:1997159792
used_memory_rss_human:1.86G
mem_fragmentation_ratio:1.86
```

mem_fragmentation_ratio 的指标，就是内存碎片率。

```shell

mem_fragmentation_ratio = used_memory_rss/ used_memory
```

**经验阀值**

1. mem_fragmentation_ratio 大于 1 但小于 1.5
2. mem_fragmentation_ratio 大于 1.5

#### 处理办法

1. 重启redis
    1. 未开启 AOF，可能会数据丢失
    2. 恢复时间过长

2. Redis 自身提供了一种内存碎片自动清理的方法
    1. ![](https://github.com/Carmenliukang/ARTS/blob/master/image/34/5.jpg)

```shell
config set activedefrag yes 
# 设置redis相关系统
# 表示内存碎片的字节数达到 100MB 时，开始清理；
active-defrag-ignore-bytes 100mb
# 表示内存碎片空间占操作系统分配给 Redis 的总空间比例达到 10% 时，开始清理。
active-defrag-threshold-lower 10
### 为了不影响其redis性能
# 表示自动清理过程所用 CPU 时间的比例不低于 25%，保证清理能正常开展；
active-defrag-cycle-min 25
# 表示自动清理过程所用 CPU 时间的比例不高于 75%，一旦超过，就停止清理，从而避免在清理时，大量的内存拷贝阻塞 Redis，导致响应延迟升高。
active-defrag-cycle-max 75
```   

***

## Share

> [0-1背包类型题目示例](https://github.com/Carmenliukang/ARTS/blob/master/week34.md#share)

### 概述

> [最低票价](https://leetcode-cn.com/problems/minimum-cost-for-tickets/)



#### 描述

在一个火车旅行很受欢迎的国度，你提前一年计划了一些火车旅行。在接下来的一年里，你要旅行的日子将以一个名为days的数组给出。每一项是一个从1到365的整数。

火车票有三种不同的销售方式：

1. 一张为期一天的通行证售价为costs[0] 美元；
2. 一张为期七天的通行证售价为costs[1] 美元；
3. 一张为期三十天的通行证售价为costs[2] 美元。

通行证允许数天无限制的旅行。 例如，如果我们在第 2 天获得一张为期 7 天的通行证，那么我们可以连着旅行 7 天：第 2 天、第 3 天、第 4 天、第 5 天、第 6 天、第 7 天和第 8 天。

返回你想要完成在给定的列表days中列出的每一天的旅行所需要的最低消费。



示例 1：

   输入：days = [1,4,6,7,8,20], costs = [2,7,15]
   输出：11
   解释： 
   例如，这里有一种购买通行证的方法，可以让你完成你的旅行计划：
   在第 1 天，你花了 costs[0] = $2 买了一张为期 1 天的通行证，它将在第 1 天生效。
   在第 3 天，你花了 costs[1] = $7 买了一张为期 7 天的通行证，它将在第 3, 4, ..., 9 天生效。
   在第 20 天，你花了 costs[0] = $2 买了一张为期 1 天的通行证，它将在第 20 天生效。
   你总共花了 $11，并完成了你计划的每一天旅行。

示例 2：
   
   输入：days = [1,2,3,4,5,6,7,8,9,10,30,31], costs = [2,7,15]
   输出：17
   解释：
   例如，这里有一种购买通行证的方法，可以让你完成你的旅行计划： 
   在第 1 天，你花了 costs[2] = $15 买了一张为期 30 天的通行证，它将在第 1, 2, ..., 30 天生效。
   在第 31 天，你花了 costs[0] = $2 买了一张为期 1 天的通行证，它将在第 31 天生效。 
   你总共花了 $17，并完成了你计划的每一天旅行。


提示：
   1 <= days.length <= 365
   1 <= days[i] <= 365
   days按顺序严格递增
   costs.length == 3
   1 <= costs[i] <= 1000

#### 分析
状态：是否出门
1. 是否出门
   如果不出门，那么dp[i]=dp[i-1]
   如果出门，那么：
      dp[i] = min(dp[i-1]+costs[0],dp[i-7]+costs[1],dp[i-30]+costs[2])
   


#### coding
```python
class Solution:
    def mincostTickets(self, days: list[int], costs: list[int]) -> int:
        max_day = days[-1]
        # 这里是为了规避不足31天的结果
        dp = [0]*(max_day+31)
        days_set = set(days)
        # 
        for i in range(max_day,-1,-1):
            if i in days_set:
                dp[i] = min(dp[i+1]+costs[0],dp[i+7]+costs[1],dp[i+30]+costs[2])
            else:
                dp[i] = dp[i+1]
        print(dp)
        return dp[0]
                
```
   









