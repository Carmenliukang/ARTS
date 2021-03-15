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