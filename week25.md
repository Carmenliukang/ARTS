# ARTS Week 25

> [![sN3VjH.jpg](https://s3.ax1x.com/2021/01/13/sN3VjH.jpg)](https://imgchr.com/i/sN3VjH)
>> 能救自己的只有自己。必须只靠自己的力量强大起来，然后超越那个事件留下的伤痕。

***

## Algoithm

> [最小路径和](https://leetcode-cn.com/problems/minimum-path-sum)

### 概述

给定一个包含非负整数的 mxn网格grid ，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

说明：每次只能向下或者向右移动一步。

示例 1： 1 3 1 1 5 1 4 2 1

    输入：grid = [[1,3,1],[1,5,1],[4,2,1]]
    输出：7
    解释：因为路径 1→3→1→1→1 的总和最小。

示例 2：

    输入：grid = [[1,2,3],[4,5,6]]
    输出：12

提示：

    m == grid.length
    n == grid[i].length
    1 <= m, n <= 200
    0 <= grid[i][j] <= 100

### 分析

可以通过将其状态分析为：

F(M,N)=min(F(M-1,N),F(M,N-1))+NUM(M,N)

现在的系统有一些小问题

### coding

```python
class Solution:
    def minPathSum(self, grid: List[List[int]]) -> int:
        if not grid or not grid[0]:
            return 0

        m = len(grid)
        n = len(grid[0])
        nums = [[0] * n for i in range(m)]

        nums[0][0] = grid[0][0]
        # 因为这里的有一些小问题
        for i in range(1, n):
            nums[0][i] = nums[0][i - 1] + grid[0][i]
        # 数据同步
        for i in range(1, m):
            nums[i][0] = nums[i - 1][0] + grid[i][0]
        # 状态转移方程式
        for i in range(1, m):
            for j in range(1, n):
                nums[i][j] = min(nums[i - 1][j], nums[i][j - 1]) + grid[i][j]

        return nums[-1][-1]
```

### 总结

1. 状态分析
2. 按照其可以走的逻辑进行修改
3. 状态转移方程式

***

## Review

> [todo](todo)

### 概述

todo

***

## Tip

> [用户故事 | 站在更高的视角看架构](https://time.geekbang.org/column/article/152196)

### 概述

学习的方法无非就是坚持，坚持，坚持 ！夯实基础，夯实基础，夯实基础！

**夯实基础，坚持下去**，这条路是最短也是最坚持的一种方式

***

## Share

> [反思](https://github.com/Carmenliukang/ARTS/blob/master/week25.md#share)

### 概述

todo