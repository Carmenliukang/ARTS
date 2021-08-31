# ARTS Week 37

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
    # todo 这里其实还可以再次优化一下，整体的内存和性能有一些低
    def canJump(self, nums: List[int]) -> bool:
        size = len(nums)

        if size == 1:
            return True

        dp = [0 for _ in range(size)]
        dp[0] = nums[0]
        for i in range(1, size):
            if dp[i - 1] >= i:
                dp[i] = max(i + nums[i], dp[i - 1])

        return dp[-1] >= size - 1

```

### 总结

通过确定最基本的 base case，那么应该就可以确定一下相关的设置

***

## Review

> [todo](todo)

### 概述

todo

***

## Tip

> [todo](todo)

### 概述

todo

***

## Share

> [todo](todo)

### 概述

todo