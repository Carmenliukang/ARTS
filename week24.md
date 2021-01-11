# ARTS Week 24

> [![s3ZUeA.jpg](https://s3.ax1x.com/2021/01/11/s3ZUeA.jpg)](https://imgchr.com/i/s3ZUeA)
>> 如果不竭尽全力到最后一刻的话，是无法取胜的。 --《刀剑神域》

***

## Algoithm

> [面试题 08.01. 三步问题](https://leetcode-cn.com/problems/three-steps-problem-lcci/submissions/)

### 概述

三步问题。有个小孩正在上楼梯，楼梯有n阶台阶，小孩一次可以上1阶、2阶或3阶。实现一种方法，计算小孩有多少种上楼梯的方式。结果可能很大，你需要对结果模1000000007。

示例1:
    
    输入：n = 3 
    输出：4 
    说明: 有四种走法 
    
示例2:
    
    输入：n = 5 
    输出：13 

提示:

n范围在[1, 1000000]之间

### 分析

经典DP问题

1. 依次写出从0开始的每一个结果对应关系
2. 将其类比成左子树/右子树 等等这些东西，我也希望能够做好。

### codeing

```python
class Solution:
    def waysToStep(self, n: int) -> int:
        if n == 1:
            return 1
        elif n == 2:
            return 2

        nums = [0 for i in range(n)]
        nums[0], nums[1], nums[2] = 1, 2, 4
        for i in range(3, n):
            nums[i] = (nums[i - 1] + nums[i - 2] + nums[i - 3]) % 1000000007

        return nums[-1]

    def waysToStep1(self, n: int) -> int:
        if n == 1:
            return 1
        elif n == 2:
            return 2

        tmp1, tmp2, tmp3 = 1, 2, 4
        for i in range(3, n):
            tmp1, tmp2, tmp3 = tmp2, tmp3, tmp1 + tmp2 + tmp3
            tmp1 = tmp1 % 1000000007
            tmp2 = tmp2 % 1000000007
            tmp3 = tmp3 % 1000000007
        return tmp3

```

### 总结

1. 将其抽象成 F(N) 只与 F(N-1) 和 F(N-2) 这些有关系，具体的 F(N-1) 与 F(N-2) 这些都已经是确定的结果。
2. 这里高度抽象后，就可以快速完成其相关的数据

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

> [DP简单总结](https://github.com/Carmenliukang/ARTS/blob/master/week24.md#share)

### 概述

> [动态规划 Leetcode](https://leetcode-cn.com/tag/dynamic-programming/)

动态规划（英语：Dynamic programming，简称 DP）是一种在数学、管理科学、计算机科学、经济学和生物信息学中使用的，通过把原问题分解为相对简单的子问题的方式求解复杂问题的方法。

动态规划常常适用于有重叠子问题和最优子结构性质的问题，动态规划方法所耗时间往往远少于朴素解法。

动态规划背后的基本思想非常简单。大致上，若要解一个给定问题，我们需要解其不同部分（即子问题），再根据子问题的解以得出原问题的解。动态规划往往用于优化递归问题，例如斐波那契数列，如果运用递归的方式来求解会重复计算很多相同的子问题，利用动态规划的思想可以减少计算量。

通常许多子问题非常相似，为此动态规划法试图仅仅解决每个子问题一次，具有天然剪枝的功能，从而减少计算量：一旦某个给定子问题的解已经算出，则将其记忆化存储，以便下次需要同一个子问题解之时直接查表。这种做法在重复子问题的数目关于输入的规模呈指数增长时特别有用。

#### 分析

1. 明确 base case -> 明确「状态」-> 明确「选择」 -> 定义 dp 数组/函数的含义。
   
2. 框架设定：

```
# 初始化 base case
dp[0][0][...] = base
# 进行状态转移
for 状态1 in 状态1的所有取值：
    for 状态2 in 状态2的所有取值：
        for ...
            dp[状态1][状态2][...] = 求最值(选择1，选择2...)
```



