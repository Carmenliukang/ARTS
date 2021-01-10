# ARTS Week 24

> [![r79Xwj.jpg](https://s3.ax1x.com/2020/12/28/r79Xwj.jpg)](https://imgchr.com/i/r79Xwj)
>> 如果不竭尽全力到最后一刻的话，是无法取胜的。 --《刀剑神域》

***

## Algoithm

> [面试题 08.01. 三步问题](https://leetcode-cn.com/problems/three-steps-problem-lcci/submissions/)

### 概述

三步问题。有个小孩正在上楼梯，楼梯有n阶台阶，小孩一次可以上1阶、2阶或3阶。实现一种方法，计算小孩有多少种上楼梯的方式。结果可能很大，你需要对结果模1000000007。

示例1:

输入：n = 3 输出：4 说明: 有四种走法 示例2:

输入：n = 5 输出：13 提示:

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

1. 将其抽象成基本的方式

***

## Review

> [todo](todo)

### 概述

***

## Tip

> [todo](todo)

### 概述

todo

***

## Share

> [todo](todo)

### 概述

TODO
