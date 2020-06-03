# ARTS Week 4

***
![图片](https://s1.ax1x.com/2020/06/03/tU2t2t.jpg)
> **凡是过往，皆为序章。 -- 莎士比亚**
***

## Algoithm
> [直接求和](https://leetcode-cn.com/problems/qiu-12n-lcof)

### 概述
求 1+2+...+n ，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。

### 分析
因为禁用了 乘除 和循环等特性，所以能够使用的就是递归的方式。同时不能使用 if 那么就需要进行 前置判断。
* 如何停止递归: n!=0
* 递归范式如何: n+fun(n-1)

### 代码
```python
# method 1
# 递归方式 ，最后会判断其 是否为 0。如果为 0，那么就不会 进行递归。
class Solution:
    def sumNums(self, n: int) -> int:
        # 判断其是否为0。
        return n != 0 and n + self.sumNums(n - 1)

# 内置函数
class Solutionm:
    def sumNums(self, n: int) -> int:
        return sum(range(n + 1))  # 这里需要注意的是需要使用 num+1
```

***
## Review
> [todo]()

