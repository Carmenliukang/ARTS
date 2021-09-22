# ARTS Week 20

> ![](https://github.com/Carmenliukang/ARTS/blob/master/image/40/douglas-rivera-_oQH-2lv6Cw-unsplash.jpg)
>> 从零开始

***

## Algoithm

> [机器人的运动范围](https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof)

### 概述

地上有一个m行n列的方格，从坐标 [0,0] 到坐标 [m-1,n-1] 。一个机器人从坐标 [0, 0]
的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]
，因为3+5+3+8=19。请问该机器人能够到达多少个格子？

示例 1：

    输入：m = 2, n = 3, k = 1
    输出：3

示例 2：

    输入：m = 3, n = 1, k = 0
    输出：1

提示：

    1 <= n,m <= 100
    0 <= k<= 20

***

### 分析

这里可以使用 DP方法，确定状态，分析转换，函数表达

#### 状态

每一个节点是否符合要求，符合为1，不符合为0

#### 转移方程式

(i-1,j || i,j-1 满足条件) && 同时当前满足条件

### code

```python

class Solution:
    def movingCount(self, m: int, n: int, k: int) -> int:
        # 这里使用的是最基础的逻辑修改
        vis = set()
        vis.add((0, 0))
        for i in range(m):
            for j in range(n):
                if ((i - 1, j) in vis or (i, j - 1) in vis) and self.digitsum(i) + self.digitsum(j) <= k:
                    vis.add((i, j))
        return len(vis)

    def digitsum(self, n):
        # 用于计算每一个位置数字之和
        ans = 0
        while n:
            ans += n % 10
            n //= 10
        return ans
```

### 总结

这个题目初期分析较为复杂，其实需要这种，M*N 的方式，其实都可以从最初的点的状态进行判断。

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

### 概述z

todo