# ARTS Week 27

> ![](https://github.com/Carmenliukang/ARTS/blob/master/image/27/27.jpg)
>> 从零开始

***

## Algoithm

> [分割回文串](https://leetcode-cn.com/problems/palindrome-partitioning/)

### 概述

给定一个字符串 s，将 s 分割成一些子串，使每个子串都是回文串。

返回 s 所有可能的分割方案。

示例:

    输入:"aab"
    输出:
    [
      ["aa","b"],
      ["a","a","b"]
    ]

### 分析

这里可以通过回溯的方式快速进行调用。

### codeing

```python
class Solution:
    def partition(self, s: str) -> list[list[str]]:
        def helper(subStr):
            i, j = 0, len(subStr) - 1
            while i <= j:
                if subStr[i] != subStr[j]:
                    return False
                i += 1
                j -= 1
            return True

        def recall(s, size, start, subset):
            if start == size:
                res.append(subset[:])
                return
            for i in range(start, size):
                if not helper(s[start:i + 1]):
                    continue
                subset.append(s[start:i + 1])
                recall(s, size, i + 1, subset)
                subset.pop()

        res = []
        size = len(s)
        recall(s, size, 0, [])
        return res

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

> [反思总结](https://github.com/Carmenliukang/ARTS/blob/master/week27.md#share)

### 概述
焦虑解决不了任何问题，只会让问题更加的严重，所以接纳自己，同时每一个小事情都做好。这样后面的路会越来越宽广。



