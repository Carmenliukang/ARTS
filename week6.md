# ARTS Week 6
>![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9zdGF0aWMwMDEuZ2Vla2Jhbmcub3JnL2luZm9xLzI2LzI2NDY1YWFmZjZkNmIzZjY2OTUxZjljZDAwOTlhZTU2LmpwZWc?x-oss-process=image/format,png)
>>仕方ないでしょ？世界は残酷なんだから 

***
## Algoithm
> [全排列](https://leetcode-cn.com/problems/permutations/)

### 概述
给定一个 没有重复 数字的序列，返回其所有可能的全排列。

    示例:
    
        输入: [1,2,3]
        输出:
        [
          [1,2,3],
          [1,3,2],
          [2,1,3],
          [2,3,1],
          [3,1,2],
          [3,2,1]
        ]


### 分析
这是一个回溯算法，需要将其每一种情况都要遍历一遍才可以。

### 代码
* 空间复杂度是 O(N)
* 时间复杂度是 O(N*N!)
```python3
class Solution:
    def permute(self, nums):
        if not nums:
            return []
        res = []

        def backtrack(nums, track=[]):
            if len(nums) == len(track):
                res.append(track[:])
                return
            for i in nums:
                if i in track:
                    continue
                track.append(i)
                backtrack(nums, track)
                # 这里需要注意的是  pop 删除最后的元素，不用清空整个 list
                track.pop()
                # 这句话是清空整个list。
                # track.clear()

        backtrack(nums)
        return res
```

### 总结
全排列算法的 模板

```
result = []
def backtrack(路径, 选择列表):
    if 满足结束条件:
        result.add(路径)
        return

    for 选择 in 选择列表:
        做选择
        backtrack(路径, 选择列表)
        撤销选择 //(这里需要注意的是从 最后开始删除相关的数据，而不是将这个 list 进行清空。这些东西是有问题的，因为系统会有各种各样的问题)
```

***
## Review
> [Building a Text Editor for a Digital-First Newsroom](https://open.nytimes.com/building-a-text-editor-for-a-digital-first-newsroom-f1cb8367fc21)

### 概述
专注于行业的特征，积极适应行业的变化，才能够让更多的人使用。
简洁，不仅仅是操作简单，更加需要的是明确。

### 思考
软件设计，在开发之前，一定要深入了解行业的一些特性，这样的产品，才是真正能够存活的产品。

***
## Tip
> [回溯算法解题套路框架](https://labuladong.gitbook.io/algo/di-ling-zhang-bi-du-xi-lie/hui-su-suan-fa-xiang-jie-xiu-ding-ban)

### 概述
废话不多说，直接上回溯算法框架。解决一个回溯问题，实际上就是一个决策树的遍历过程。你只需要思考 3 个问题：
1、路径：也就是已经做出的选择。
2、选择列表：也就是你当前可以做的选择。
3、结束条件：也就是到达决策树底层，无法再做选择的条件。

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


***
## Share
> [MySQL是怎么保证数据不丢的？](https://time.geekbang.org/column/article/76161)


### 概述
1. binlog redolog buffer 机制
2. binlog cache 线程自我维护
3. redolog cache 全局通用
