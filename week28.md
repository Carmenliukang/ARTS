# ARTS Week 28

> ![](https://github.com/Carmenliukang/ARTS/blob/master/image/28/1.jpg)
>> 一星陨落，黯淡不了星空灿烂；一花凋零，荒芜不了整个春天。
> > ——巴尔扎克

***

## Algoithm

> [元素和小于等于阈值的正方形的最大边长](https://leetcode-cn.com/problems/maximum-side-length-of-a-square-with-sum-less-than-or-equal-to-threshold)

### 概述

给你一个大小为=m x n=的矩阵=mat=和一个整数阈值=threshold。

请你返回元素总和小于或等于阈值的正方形区域的最大边长；如果没有这样的正方形区域，则返回 0=。

示例 1：

    输入：mat = [[1,1,3,2,4,3,2],[1,1,3,2,4,3,2],[1,1,3,2,4,3,2]], threshold = 4 
    输出：2 
    解释：总和小于 4 的正方形的最大边长为 2，
    如图所示。

示例 2：

    输入：mat = [[2,2,2,2,2],[2,2,2,2,2],[2,2,2,2,2],[2,2,2,2,2],[2,2,2,2,2]], threshold = 1 
    输出：0

示例 3：

    输入：mat = [[1,1,1,1],[1,0,0,0],[1,0,0,0],[1,0,0,0]], threshold = 6 
    输出：3

示例 4：

    输入：mat = [[18,70],[61,1],[25,85],[14,40],[11,96],[97,96],[63,45]], threshold = 40184 
    输出：2

提示：

* 1 <= m, n <= 300
* m == mat.length
* n == mat[i].length
* 0 <= mat[i][j] <= 10000
* 0 <= threshold=<= 10^5

### 分析

1. 这前缀和方式进行统计，以(m,n) 为终点的总和

### coding

```python

class Solution:
    def maxSideLength(self, mat: list[list[int]], threshold: int) -> int:
        m = len(mat)
        n = len(mat[0])

        dp = [[0] * (n + 1) for i in range(m + 1)]

        for i in range(1, m + 1):
            for j in range(1, n + 1):
                # 这里使用mat[i-1][j-1] 是因为 mat 数组下标是从 -1 开始的。
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1] - dp[i - 1][j - 1] + mat[i - 1][j - 1]

        def getRect(x1, y1, x2, y2):
            return dp[x2][y2] - dp[x1 - 1][y2] - dp[x2][y1 - 1] + dp[x1 - 1][y1 - 1]

        # 使用二分法的方式进行查询，因为这里会相比较会好一些
        l, r, ans = 1, min(m, n), 0
        while l <= r:
            mid = (l + r) // 2
            find = any(getRect(i, j, i + mid - 1, j + mid - 1) <= threshold for i in range(1, m - mid + 2) for j in
                       range(1, n - mid + 2))
            if find:
                ans = mid
                l = mid + 1
            else:
                r = mid - 1
        return ans

```

***

## Review

> [Redis-有一亿个keys要统计，应该用哪种集合？](https://time.geekbang.org/column/article/280680)

### 概述

常见的统计方式如下：

1. 聚合统计
2. 排序统计
3. 二值状态统计
4. 基数统计

#### 聚合统计

所谓的聚合统计，就是指统计多个集合元素的聚合结果，包括：统计多个集合的共有元素（交集统计）；把两个集合相比，统计其中一个集合独有的元素（差集统计）；统计多个集合的所有元素（并集统计）。



***

## Tip

> [linux文件实时同步](https://www.cnblogs.com/zzf0305/p/9319962.html)
>
> [rsync-详细说明](https://rsync.samba.org/)

### 概述

使用 rsync 自动同步文件修改后的文件

1. 安装

```shell

# Debian
sudo apt-get install rsync

# Red Hat
sudo yum install rsync

# Arch Linux
sudo pacman -S rsync

```

2. 基本用法

    1. -r 递归同步所有文件
       ```shell
          raync -r source(源目录) destination(目标目录)
       ```
    2. -a 递归同步所有文件，并且包含 元信息
       ```shell
          raync -a source(源目录) destination(目标目录)
       ```
    3. -n 模拟执行命令的结果
       ```shell
          raync -anv source(源目录) destination(目标目录)
       ```
    4. --delete 另一个目录作为镜像，将不存在这个目录的文件删除
       ```shell
          rsync -av --delete source/ destination
       ```
    5. --exclude
    6. --include

3. 文件同步协议
    1. ssh
    2. rsync

4. 增量备份
    1. --link-dest

***

## Share

> [前缀和类型题目总结](https://github.com/Carmenliukang/ARTS/blob/master/week28.md#share)

### 概述

前缀和类型题目总结

相似题目如下：

[矩阵区域和](https://leetcode-cn.com/problems/matrix-block-sum)

[元素和小于等于阈值的正方形的最大边长](https://leetcode-cn.com/problems/maximum-side-length-of-a-square-with-sum-less-than-or-equal-to-threshold)

### 前缀和

本题需要用到一些二维前缀和（Prefix Sum）的知识，它是一维前缀和的延伸：

设二维数组 A 的大小为 m * n，行下标的范围为 [1, m]，列下标的范围为 [1, n]。

数组 P 是 A 的前缀和数组，等价于 P 中的每个元素 P[i][j]：

如果 i 和 j 均大于 0，那么 P[i][j] 表示 A 中以 (1, 1) 为左上角，(i, j) 为右下角的矩形区域的元素之和；

如果 i 和 j 中至少有一个等于 0，那么 P[i][j] 也等于 0。

数组 P 可以帮助我们在 O(1)O(1) 的时间内求出任意一个矩形区域的元素之和。具体地，设我们需要求和的矩形区域的左上角为 (x1, y1)，右下角为 (x2, y2)，则该矩形区域的元素之和可以表示为：

sum = A[x1..x2][y1..y2] = P[x2][y2] - P[x1 - 1][y2] - P[x2][y1 - 1] + P[x1 - 1][y1 - 1]

其正确性可以通过容斥原理得出。以下图为例，当 A 的大小为 8 * 5，需要求和的矩形区域（深绿色部分）的左上角为 (3, 2)，右下角为 (5, 5) 时，该矩形区域的元素之和为 P[5][5] - P[2][5] - P[5][1] +
P[2][1]。

![](https://github.com/Carmenliukang/ARTS/blob/master/image/28/2.png)

那么如何得到数组 P 呢？我们按照行优先的顺序依次计算数组 P 中的每个元素，即当我们在计算 P[i][j] 时，数组 P 的前 i - 1 行，以及第 i 行的前 j - 1 个元素都已经计算完成。此时我们可以考虑 (i, j) 这个 1

* 1 的矩形区域，根据上面的等式，有：

```
A[i][j] = P[i][j] - P[i - 1][j] - P[i][j - 1] + P[i - 1][j - 1]
```

由于等式中的 A[i][j]，P[i - 1][j]，P[i][j - 1] 和 P[i - 1][j - 1] 均已知，我们可以通过：

```
P[i][j] = P[i - 1][j] + P[i][j - 1] - P[i - 1][j - 1] + A[i][j]
```

在 O(1)O(1) 的时间计算出 P[i][j]。因此按照行优先的顺序，我们可以在 O(MN)O(MN) 的时间得到数组 P。在此之后，我们就可以很方便地在 O(1)O(1) 的时间内求出任意一个矩形区域的元素之和了。

