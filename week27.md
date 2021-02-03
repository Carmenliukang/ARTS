# ARTS Week 27

> ![](https://github.com/Carmenliukang/ARTS/blob/master/image/27/1.jpg)
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

> [“万金油”的String，为什么不好用了？](https://time.geekbang.org/column/article/279649)

### 概述

string 是redis 支持的一种存储的结构

缺点：

1. 就是它保存数据时所消耗的内存空间较多。

#### 简单动态字符串（Simple Dynamic String，SDS）结构体

![](https://github.com/Carmenliukang/ARTS/blob/master/image/27/2.jpg)

* buf：字节数组，保存实际数据。为了表示字节数组的结束，Redis 会自动在数组最后加一个“\0”，这就会额外占用 1 个字节的开销。
* len：占 4 个字节，表示 buf 的已用长度。
* alloc：也占个 4 字节，表示 buf 的实际分配长度，一般大于 len。

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



