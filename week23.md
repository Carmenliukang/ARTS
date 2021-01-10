# ARTS Week 23

> [![r79Xwj.jpg](https://s3.ax1x.com/2020/12/28/r79Xwj.jpg)](https://imgchr.com/i/r79Xwj)
>> 如果不竭尽全力到最后一刻的话，是无法取胜的。 --《刀剑神域》

***

## Algoithm

> [前序遍历构造二叉搜索树](https://leetcode-cn.com/problems/construct-binary-search-tree-from-preorder-traversal)

### 概述

返回与给定前序遍历preorder 相匹配的二叉搜索树（binary search tree）的根结点。

(回想一下，二叉搜索树是二叉树的一种，其每个节点都满足以下规则，对于node.left的任何后代，值总 < node.val，而 node.right 的任何后代，值总 > node.val。此外，前序遍历首先显示节点node
的值，然后遍历 node.left，接着遍历 node.right。）

题目保证，对于给定的测试用例，总能找到满足要求的二叉搜索树。

    示例：
    
    输入：[8,5,1,7,10,12]
    输出：[8,5,10,1,7,null,12]

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/03/08/1266.png)

提示：

1. 1 <= preorder.length <= 100
2. 1 <= preorder[i]<= 10^8
3. preorder 中的值互不相同

### 分析

使用二叉搜索树的特性，依次进行递归生成

### codeing

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:
    def bstFromPreorder(self, preorder: list[int]) -> TreeNode:
        self.ids = 0
        self.preorder = preorder
        return self.dfs(float("-inf"), float("inf"))

    def dfs(self, low, upper):
        if self.ids == len(self.preorder):
            return None

        val = self.preorder[self.ids]
        if val < low or val > upper:
            return None

        root = TreeNode(val)
        self.ids += 1
        root.left = self.dfs(low, val)
        root.right = self.dfs(val, upper)
        return root

```

### 总结

1. 忽略左右子树，将其看成一个单点去思考设计代码。
2. 递归的方式完成相关的数据同步

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

> [保持情绪的稳定](https://github.com/Carmenliukang/ARTS/blob/master/week23.md#share)

### 概述

> [如何进行情绪管理](https://www.zhihu.com/question/20789554/answer/132621010)

情绪管理的本质其实就是：**在理解和完全接纳自己情绪的前提下，能够用理性去思考和控制自己的行动。**

#### 误解点
1. 情绪管理=**负面**管理
    1. 你如果控制不了自己的狂喜，那你同样也控制不了自己的狂怒。
2. 情绪管理=情绪**压制**
    1. 有不少的人将情绪管理理解为怎样将自己的情绪给“压”住。
    2. 他们想要得到的是一种外在的、可以将他们自身的情绪“打败”的力量。 或是希望能够有一种方法将他们的情绪消除。
    

#### 认知
1. 情绪管理即如同治水，水非无根，自然不可能凭空消失；水不可堵，愈堵愈生灾祸。
2. 情绪不会消失，也无法被压制，你必须理解和接纳它；

#### 目的
1. 你切断了你的「情绪系统」对你的思考和选择的影响权限。
2. 做情绪管理的最根本的目的：不让情绪影响到决策。


#### 分析
1. 面对和感受你的情绪。
    1. 绝大多数的「情绪问题」之所以会是个「问题」，其最根本的原因就是在于人们对于自身情绪的阻抗。
    2. 一个是对外界异常的提醒 
    3. 一个是对内在异常的提醒
    
2. 分析你的情绪
    1. 自动化思维，无意识的条件反射
    2. 理智思考思维
3. 用理性思考。
    1. 你能够学会在情绪激动时切断情绪对你决策系统的影响权限；
    2. 你能够学会不管你多么悲伤，多么愤怒，多么恐惧，你的行动和选择都是由理性思维所做出的
    3. 你能够意识到情绪的激动和理智的决断在本质上是两回事，任何时候你的决策系统都将情绪因素隔绝在外；
    
#### 方法
1. 情绪体会练习。
2. 情绪分析练习。
    1. 吸纳子啊的情绪和感受是什么？
    2. 诱因是什么？
    3. 感觉是否合理？信念是否合理？
    
3. 理智思维练习

#### 总结
在自我提升道路上最大的敌人就是**急躁的立刻就想得到反馈**、迫切的马上想获得改变，因此不管多么有效的方法很多人都是坚持不到一两周就放弃了。

**No pains，no gains。**
