_# ARTS Week 37

> ![](https://github.com/Carmenliukang/ARTS/blob/master/image/37/1.jpg)
>> 从零开始

***

## Algoithm

> [跳跃游戏](https://leetcode-cn.com/problems/jump-game)

### 概述

给定一个非负整数数组 nums ，你最初位于数组的 第一个下标 。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个下标。

示例 1：

    输入：nums = [2,3,1,1,4]
    输出：true
    解释：可以先跳 1 步，从下标 0 到达下标 1, 然后再从下标 1 跳 3 步到达最后一个下标。

示例 2：

    输入：nums = [3,2,1,0,4]
    输出：false
    解释：无论怎样，总会到达下标为 3 的位置。但该下标的最大跳跃长度是 0 ， 所以永远不可能到达最后一个下标。

提示：

    1 <= nums.length <= 3 * 104
    0 <= nums[i] <= 105

### 分析

常规套路，先确定base case>>> 状态 >>> 选择 >>> 函数/类

* base case
    * F(0)=nums[0]
* 状态
    * 当前位置能够到达的最远距离
* 选择
    * 能够到达当前位置
    * 可以到达当前位置：Max(F(N-1),i+nums[i])
* 函数/类
    * 确定基础函数选择

### code

```python3

from typing import List


class Solution:
    def canJump(self, nums: List[int]) -> bool:
        # todo 这里其实还可以再次优化一下，整体的内存和性能有一些低
        size = len(nums)

        if size == 1:
            return True

        dp = [0 for _ in range(size)]
        dp[0] = nums[0]
        for i in range(1, size):
            if dp[i - 1] >= i:
                dp[i] = max(i + nums[i], dp[i - 1])
        # 最后 注意点是 需要 N-1 步骤
        return dp[-1] >= size - 1

    def canJumpMethod(self, nums: List[int]) -> bool:
        # 这里部分使用 状态压缩，所以内存会节省很多，同时如果判断这个最远距离超过size-1，那么也说明一定可以跳到
        size = len(nums)

        if size == 1:
            return True

        max_num = nums[0]

        for i in range(1, size):
            if max_num >= i:
                max_num = max(i + nums[i], max_num)
                if max_num >= size - 1:
                    return True

        return False

```

### 总结

通过确定最基本的 base case，那么就应该能够确定状态，然后就可以选择。

***

## Review

> [todo](todo)

### 概述

todo

***

## Tip

> [为什么还会有kill 不掉的语句](https://time.geekbang.org/column/article/79026)

### 概述

kill 中会有kill 时间较长，导致的数据出现问题

#### kill 类型

1. kill query + 线程id
2. kill connection + 线程id

但是中间有可能会出现 killed 这种情况

具体的kill 流程

![](https://github.com/Carmenliukang/ARTS/blob/master/image/37/2.webp)

#### 收到 kill 以后，线程做什么？

1. kill 并不是马上停止，而是告诉执行线程说：这条语句已经不需要执行了，可以开始 "执行停止的逻辑"
    1. 把 session B 的运行状态改成 THD::KILL_QUERY(将变量 killed 赋值为 THD::KILL_QUERY)；
    2. 给 session B 的执行线程发一个信号。

#### 展示其他的现象

1. set global innodb_thread_concurrency=2
2. 执行如下操作：
    1. ![](https://github.com/Carmenliukang/ARTS/blob/master/image/37/3.webp)
    2. 显示结果：
        1. sesssion C 执行的时候被堵住了；
        2. 但是 session D 执行的 kill query C 命令却没什么效果，
        3. 直到 session E 执行了 kill connection 命令，才断开了 session C 的连接，提示“Lost connection to MySQL server during query”，
        4. 但是这时候，如果在 session E 中执行 show processlist，你就能看到下面这个图。
        5. ![](https://github.com/Carmenliukang/ARTS/blob/master/image/37/4.webp)

***

## Share

> [DP 方法论总结](https://github.com/Carmenliukang/ARTS/blob/master/week37.md#share)

### 概述

相关题目总结：

> [跳跃游戏](https://leetcode-cn.com/problems/jump-game)
>
> [跳跃游戏 II](https://leetcode-cn.com/problems/jump-game-ii/)

### 分析

首先确定状态，然后选择，那么这个选择是可以顺序性，例如：

1. [最佳买卖股票时机含冷冻期](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)
    1. 固定顺序选择：买入 卖出 冷冻期 三种状态
2. [跳跃游戏 II](https://leetcode-cn.com/problems/jump-game-ii/)
    1. 向前选择
3. [买卖股票的最佳时机 II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)
    1. 与之前的数据比较选择，这种也是大部分的