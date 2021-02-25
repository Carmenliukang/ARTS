# ARTS Week 29

> ![](https://github.com/Carmenliukang/ARTS/blob/master/image/29/1.jpeg)
>> 他死在一个谁都不知道角落我不想他的骨灰毫无意义。

***

## Algoithm

> [递增顺序查找树](https://leetcode-cn.com/problems/increasing-order-search-tree/)

### 概述

给你一个树，请你 按中序遍历 重新排列树，使树中最左边的结点现在是树的根，并且每个结点没有左子结点，只有一个右子结点。

示例 ：

    输入：[5,3,6,2,4,null,8,1,null,null,null,7,9]
    
           5
          / \
        3    6
       / \    \
      2   4    8
     /        / \ 
    1        7   9
    
    输出：[1,null,2,null,3,null,4,null,5,null,6,null,7,null,8,null,9]
    
     1
      \
       2
        \
         3
          \
           4
            \
             5
              \
               6
                \
                 7
                  \
                   8
                    \
                     9  

提示：

给定树中的结点数介于 1 和 100 之间。

每个结点都有一个从 0 到 1000 范围内的唯一整数值。

### 分析

可以使用两种方式处理：

1. 中序遍历+生成树
2. 中序遍历+节点同步

### coding

```python


# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:
    def __init__(self):
        self.res = TreeNode()
        self.ans = self.res

    def increasingBST(self, root: TreeNode) -> TreeNode:
        self.dfs(root)
        return self.res.right

    def dfs(self, root):
        if root is None:
            return

        self.dfs(root.left)
        root.left = None
        self.ans.right = root
        self.ans = root
        self.dfs(root.right)


```

***

## Review

> [How to Stand Out as a Software Engineer in 2021 — Insights & Advice](https://simon-holdorf.medium.com/how-to-stand-out-as-a-software-engineer-in-2021-insights-advice-803016bf8d38)

### 概述

1. The ability to code is not enough
    1. To become a software engineer in 2021, you have to communicate your ideas and solutions.

2. Be a good team player but don’t shy away from challenging problems
    1. Although working as part of a team is essential, it’s also important not to shy away from challenging problems.

3. Be creative and inventive

4. Be a product person

5. Learn new programming languages and frameworks

6. Explore different areas of technology development

***

## Tip

> [Redis-GEO](https://time.geekbang.org/column/article/281745)

### 概述

GEO 是扩展数据类型

#### 面向 LBS 应用的 GEO 数据类型 （Location-Based Service，LBS）

使用 hash 保存

![](https://github.com/Carmenliukang/ARTS/blob/master/image/29/2.jpg)

Sorted Set 方式

![](https://github.com/Carmenliukang/ARTS/blob/master/image/29/3.jpg)

GeoHash 编码方式

1. 二分区间，区间编码 :
    1. 当我们要对一组经纬度进行 GeoHash 编码时，我们要先对经度和纬度分别编码，然后再把经纬度各自的编码组合成一个最终编码。

分析

![](https://github.com/Carmenliukang/ARTS/blob/master/image/29/4.jpg)

![](https://github.com/Carmenliukang/ARTS/blob/master/image/29/5.jpg)

![](https://github.com/Carmenliukang/ARTS/blob/master/image/29/6.jpg)

***

## Share

> [0-1背包类型题目总结](https://github.com/Carmenliukang/ARTS/blob/master/week29.md#share)

### 概述

给你一个可装载重量为W的背包和N个物品，每个物品有重量和价值两个属性。其中第i个物品的重量为wt[i]，价值为val[i]，现在让你用这个背包装物品，最多能装的价值是多少？

```
N = 3, W = 4
wt = [2, 1, 3]
val = [4, 2, 3]
```

算法返回 6，选择前两件物品装进背包，总重量 3 小于W，可以获得最大价值 6。

1. 「状态」和「选择」
    1. 状态：如何才能描述现在的问题局面？
        1. 背包的容量
        2. 可选择的物品

2. dp 数组
    1. 二维数组
    2. 物品 当前最大总容量为 W

3. 选择

```
int dp[N+1][W+1]
dp[0][..] = 0
dp[..][0] = 0

for i in [1..N]:
    for w in [1..W]:
        dp[i][w] = max(
            把物品 i 装进背包,
            不把物品 i 装进背包
        )
return dp[N][W]
```

4. 伪代码>代码

```
int knapsack(int W, int N, vector<int>& wt, vector<int>& val) {
    // vector 全填入 0，base case 已初始化
    vector<vector<int>> dp(N + 1, vector<int>(W + 1, 0));
    for (int i = 1; i <= N; i++) {
        for (int w = 1; w <= W; w++) {
            if (w - wt[i-1] < 0) {
                // 当前背包容量装不下，只能选择不装入背包
                dp[i][w] = dp[i - 1][w];
            } else {
                // 装入或者不装入背包，择优
                dp[i][w] = max(dp[i - 1][w - wt[i-1]] + val[i-1], 
                               dp[i - 1][w]);
            }
        }
    }

    return dp[N][W];
}
```
