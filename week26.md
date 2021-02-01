# ARTS Week 26

> ![](https://github.com/Carmenliukang/ARTS/blob/master/image/26/26.jpg)
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

> [切片集群：数据增多了，是该加内存还是加实例？](https://time.geekbang.org/column/article/276545)

### 概述

方向分为两种：

1. 纵向扩展
2. 横向扩展

![](https://github.com/Carmenliukang/ARTS/blob/master/image/26/redis-扩展.jpg)


***

## Tip

> [coder2gwy](https://github.com/coder2gwy/coder2gwy)

### 概述

程序员上岸经历资料

***

## Share

> [回溯算法总结](https://github.com/Carmenliukang/ARTS/blob/master/week26.md#share)

### 概述

*解决一个回溯问题，实际上就是一个决策树的遍历过程*

1. 路径：也就是已经做出的选择。
2. 选择列表：也就是你当前可以做的选择。
3. 结束条件：也就是到达决策树底层，无法再做选择的条件。

#### 框架

```
result = []
def backtrack(路径, 选择列表):
    if 满足结束条件:
        result.add(路径)
        return
    
    for 选择 in 选择列表:
        做选择
        backtrack(路径, 选择列表)
        撤销选择
```

其核心就是 for 循环里面的递归，在递归调用之前「做选择」，在递归调用之后「撤销选择」

#### 例子

##### N皇后

这个问题很经典了，简单解释一下：给你一个 N×N 的棋盘，让你放置 N 个皇后，使得它们不能互相攻击。

PS：皇后可以攻击同一行、同一列、左上左下右上右下四个方向的任意单位。

```java

vector<vector<string>> res;

/* 输入棋盘边长 n，返回所有合法的放置 */
vector<vector<string>> solveNQueens(int n) {
    // '.' 表示空，'Q' 表示皇后，初始化空棋盘。
    vector<string> board(n, string(n, '.'));
    backtrack(board, 0);
    return res;
}

// 路径：board 中小于 row 的那些行都已经成功放置了皇后
// 选择列表：第 row 行的所有列都是放置皇后的选择
// 结束条件：row 超过 board 的最后一行
void backtrack(vector<string>& board, int row) {
    // 触发结束条件
    if (row == board.size()) {
        res.push_back(board);
        return;
    }
    
    int n = board[row].size();
    for (int col = 0; col < n; col++) {
        // 排除不合法选择
        if (!isValid(board, row, col)) 
            continue;
        // 做选择
        board[row][col] = 'Q';
        // 进入下一行决策
        backtrack(board, row + 1);
        // 撤销选择
        board[row][col] = '.';
    }
}

/* 是否可以在 board[row][col] 放置皇后？ */
bool isValid(vector<string>& board, int row, int col) {
    int n = board.size();
    // 检查列是否有皇后互相冲突
    for (int i = 0; i < n; i++) {
        if (board[i][col] == 'Q')
            return false;
    }
    // 检查右上方是否有皇后互相冲突
    for (int i = row - 1, j = col + 1; 
            i >= 0 && j < n; i--, j++) {
        if (board[i][j] == 'Q')
            return false;
    }
    // 检查左上方是否有皇后互相冲突
    for (int i = row - 1, j = col - 1;
            i >= 0 && j >= 0; i--, j--) {
        if (board[i][j] == 'Q')
            return false;
    }
    return true;
}

```

![](https://github.com/labuladong/fucking-algorithm/blob/master/pictures/backtracking/7.jpg)







