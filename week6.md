# ARTS Week 6
>![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9zdGF0aWMwMDEuZ2Vla2Jhbmcub3JnL2luZm9xLzI2LzI2NDY1YWFmZjZkNmIzZjY2OTUxZjljZDAwOTlhZTU2LmpwZWc?x-oss-process=image/format,png)
>>这个世界是残酷的

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



