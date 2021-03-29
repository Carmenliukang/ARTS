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
        dp = [float("inf")] * (amount+1)

        dp[0] = 0

        for i in range(1,amount+1):
            for coin in coins:
                if i>=coin:
                    dp[i] = min(dp[i],dp[i-coin]+1)
        
        return dp[amount] if dp[amount]!=float("inf") else -1
            

```

***

## Review

> [todo](todo)

### 概述

todo

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

> [todo](todo)

### 概述

todo








