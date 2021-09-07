# ARTS Week 38

> ![](https://github.com/Carmenliukang/ARTS/blob/master/image/38/1.jpeg)
>> 从零开始

***

## Algoithm

> [单词拆分](https://leetcode-cn.com/problems/word-break)

### 概述

给定一个非空字符串 s 和一个包含非空单词的列表 wordDict，判定s是否可以被空格拆分为一个或多个在字典中出现的单词。

说明：

拆分时可以重复使用字典中的单词。 你可以假设字典中没有重复的单词。

示例 1：

    输入: s = "leetcode", wordDict = ["leet", "code"]
    输出: true 
    解释: 返回 true 因为 "leetcode" 可以被拆分成 "leet code"。

示例 2：

    输入: s = "applepenapple", wordDict = ["apple", "pen"]
    输出: true 
    解释: 返回 true 因为 "applepenapple" 可以被拆分成 "apple pen apple"。 注意你可以重复使用字典中的单词。

示例 3：

    输入: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
    输出: false

### 分析

这种判断是否可以组合，都是一种极值判断，大概的方向是递归问题。

那么接下来还是老三步：

1. 状态
2. 选择
3. 函数

#### 状态

首先这里的状态可以从问题入手，其中需要到最后是否满足，那么其中每一都可以判断是否满足，这样状态就变成了，从0-i 中，wordDict 是否满足？

#### 选择

1. 特殊情况
    1. 如果均为空，那么为空的情况，结果肯定为真。
2. 多种选择
    1. 0-i 判断
    2. 中间增加一个 j，从0-j，j-i 均满足，得到转换 dp[i]=dp[j] && check(s[j..i−1])

#### 函数

1. dp 定义
2. 状态选择
3. 返回结果

### code

```python3

from typing import List


class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        # 状态 当前i,j 是否为0
        # 选择 0-i 中进行切割，0-j,j-i 中可以切割。
        size = len(s)
        dp = [False] * (size + 1)
        dp[0] = True
        # 使用哈希表快速同步
        word_map = {i: True for i in wordDict}
        # 使用dp算法，进行计算
        for i in range(1, size + 1):
            for j in range(i):
                if dp[j] and word_map.get(s[j:i]):
                    dp[i] = True
                    break
        return dp[-1]

```

### 总结

1. 关于状态
    1. 动态问题 可以从问题入手，问题的最终判断即为单个的状态
2. 关于选择
    1. 选择可以通过多层循环完成，从中的状态进行选择

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