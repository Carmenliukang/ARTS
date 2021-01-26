# ARTS Week 26

> [![s7OOHO.jpg](https://s3.ax1x.com/2021/01/23/s7OOHO.jpg)](https://imgchr.com/i/s7OOHO)
>> 静水流深，沧笙踏歌

***

## Algoithm

> [可被三整除的最大和](https://leetcode-cn.com/problems/greatest-sum-divisible-by-three/)

### 概述

给你一个整数数组 nums，请你找出并返回能被三整除的元素最大和。

示例 1：

    输入：nums = [3,6,5,1,8]
    输出：18 
    解释：选出数字 3, 6, 1 和 8，它们的和是 18（可被 3 整除的最大和）。 

示例 2：

    输入：nums = [4]
    输出：0 
    解释：4 不能被 3 整除，所以无法选出数字，返回 0。 

示例 3：

    输入：nums = [1,2,3,4,4]
    输出：12 
    解释：选出数字 1, 3, 4 以及 4，它们的和是 12（可被 3 整除的最大和）。

提示：

    1 <= nums.length <= 4 * 10^4 
    1 <= nums[i] <= 10^4

### 分析

状态：

1. %3 以后，就有三种情况：
    1. ==0
    2. ==1
    3. ==2

2. 依次定义其状态，然后将其相关转化

### coding

```python
class Solution:
    def maxSumDivThree(self, nums: list[int]) -> int:
        size = len(nums)
        dp = [[0] * 3 for i in range(len(nums) + 1)]
        dp[0] = [0, float("-inf"), float("-inf")]

        for i in range(1, size + 1):
            if nums[i - 1] % 3 == 0:
                dp[i][0] = max(dp[i - 1][0], dp[i - 1][0] + nums[i - 1])
                dp[i][1] = max(dp[i - 1][1], dp[i - 1][1] + nums[i - 1])
                dp[i][2] = max(dp[i - 1][2], dp[i - 1][2] + nums[i - 1])
            elif nums[i - 1] % 3 == 1:
                dp[i][0] = max(dp[i - 1][0], dp[i - 1][2] + nums[i - 1])
                dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] + nums[i - 1])
                dp[i][2] = max(dp[i - 1][2], dp[i - 1][1] + nums[i - 1])
            elif nums[i - 1] % 3 == 2:
                dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] + nums[i - 1])
                dp[i][1] = max(dp[i - 1][1], dp[i - 1][2] + nums[i - 1])
                dp[i][2] = max(dp[i - 1][2], dp[i - 1][0] + nums[i - 1])
        return dp[size][0]
```

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