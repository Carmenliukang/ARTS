`# ARTS Week 36

> ![](https://github.com/Carmenliukang/ARTS/blob/master/image/36/1.jpg)
>> 从零开始

***

## Algoithm

> [买卖股票的最佳时机 II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)

### 概述

给定一个数组 prices ，其中prices[i] 是一支给定股票第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。

注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

示例 1:

    输入: prices = [7,1,5,3,6,4]
    输出: 7
    解释: 在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
        随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 。

示例 2:

    输入: prices = [1,2,3,4,5]
    输出: 4
    解释: 在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
        注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。

示例3:

    输入: prices = [7,6,4,3,1]
    输出: 0
    解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。

提示：

1 <= prices.length <= 3 * 104

0 <= prices[i] <= 104

来源：力扣（LeetCode）

链接：https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

#### 分析

这个题目是一个动态规划问题，所以还是按照老一套流程：

1. base case
2. 状态
3. 选择
4. 函数/类

##### base case

[0,-prices[0]]

##### 状态

* 当前持有股票
* 当前没有股票

##### 选择

当前持有股票 = Max(昨天未持有-prices[i],昨天持有的股票)
当前没有股票 = Max(昨天未持有,昨天持有股票+prices[i])

##### 函数/类

具体的实现

#### code

```python

from typing import List


class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        # 使用动态规划的方式更加深入同步数据
        size = len(prices)
        dp = [[0, 0]] * size
        dp[0][0] = 0
        dp[0][1] = -prices[0]
        # 状态 当前 是否 有股票
        # 选择 有股票 ： max(继续持有，重新购买)
        #      无股票：max(继续不持有，卖出股票)
        for i in range(1, size):
            dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] + prices[i])
            dp[i][1] = max(dp[i - 1][0] - prices[i], dp[i - 1][1])
        return max(dp[-1])

    def maxProfitFail(self, prices: List[int]) -> int:
        # 这个时间超时了
        size = len(prices)
        dp = [0] * size
        for i in range(1, size):
            mid = float("-inf")
            for j in range(i):
                mid = max(dp[j] + prices[i] - prices[j], mid)
            dp[i] = max(dp[i - 1], mid)
        return dp[-1]

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

todo`