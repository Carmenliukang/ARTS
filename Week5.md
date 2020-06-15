# ARTS Week 5
***
![图片](https://s1.ax1x.com/2020/06/15/Np7gvq.jpg)
> **自己来选择，不会后悔的路。** 

***
## Algoithm
> [二叉树换成累加树](https://leetcode-cn.com/problems/convert-bst-to-greater-tree)

### 概述
给定一个二叉搜索树（Binary Search Tree），把它转换成为累加树（Greater Tree)，使得每个节点的值是原来的节点值加上所有大于它的节点值之和。

例如：

    输入: 原始二叉搜索树:
                  5
                /   \
               2     13
    
    输出: 转换为累加树:
                 18
                /   \
              20     13

### 分析
二叉树类型的算法题目，整体的规律就是 

* 前序遍历
* 中序遍历
* 后续遍历

二叉树的问题就是按照这三种方式进行递归遍历，最后得到相应的结果，就像这个题目一样，就是后续遍历，因为需要先将右边的结果相加，然后再次进行将结果设计成左节点。

### 代码
```python

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None


class Solution:

    def __init__(self):
        self.total = 0

    def convertBST(self, root: TreeNode) -> TreeNode:
        if root != None:
            # 显示右子树 遍历
            self.convertBST(root.right)
            self.total += root.val
            root.val = self.total
            self.convertBST(root.left)

        return root
```

***
## Review
> [工作中可以规避的6种低效方式](https://levelup.gitconnected.com/6-programming-habits-that-make-you-an-ineffective-programmer-aa4aac64fc4e)

### 概述
1. 多会议 or 长会议
    * 尽可能的保证事情的连续性，从而提高效率。
2. 过度优化
    * 为了解决可能发生的问题，增加的代码，很可能未来都用不到
    * 同时因为有可能存在其他的问题。
3. 重复造轮子
    * 使用框架自有轮子，一方面 代表 已经经过了验证，同时也代表能够快速开发。
4. 一致性
    * 需求的一致性，一定要保证需求的确定，或者修改后，需要及时快速调整才可以。
5. 无计划
    * 不论看框架源码、设计软件，都是需要从较高维度思考设计，这样才能有自己的价值和架构设计。
6. 不寻求帮助
    * 寻求帮助 和 能力不足 是两个不无关联的事情，因为多和同学尝试沟通，这样也是有可能了解更加多的业务。

### 思考
确实工作中需要考虑很多的问题，比如技术选型、框架设计、开发周期，等这些因素，同时往往影响进度的不是代码，而是需求等情况的变动，
所以一方面需要的是留足周期变动的时间，还需要不断的调整沟通，实现快速的响应，这样才能够保证系统的按时开发上线。

***
## Tip
>[Hystrix 限流、熔断、降级 技术解析(非源码结构)](https://www.jianshu.com/p/3e11ac385c73) 
>
>[Hystrix GitHub](https://github.com/Netflix/Hystrix/wiki)

### 概述
本文主要讲述了 Hystrix 限流、熔断 部分逻辑。
1. 限流：线程隔离 or 信号量
    * 线程隔离：业务请求和处理请求是分开，同时保证并发量。
    * 信号量：业务请求和处理请求是同一个线程。
2. 熔断：如果一段时间内，请求错误超过一定的百分比，那么就会自动将其熔断，同时，也会定时，尝试少量请求，查看请求是否正常。


### 思考
如果需要从整体的高度来看，通过控制其部分服务的访问，使问题影响范围限制在较小范围，这是一种非常不错的方式。

***
## Share
> [动态规划问题的套路](https://labuladong.gitbook.io/algo/dong-tai-gui-hua-xi-lie/dong-tai-gui-hua-xiang-jie-jin-jie)

### 概述
这里简单的描述了 动态规划的 一些 算法套路：
* 存在「重叠子问题」
* 具备「最优子结构」
* **正确的「状态转移方程」**

**最终套路**
* 明确 base case -> 明确「状态」-> 明确「选择」 -> 定义 dp 数组/函数的含义。
```
# 初始化 base case
dp[0][0][...] = base
# 进行状态转移
for 状态1 in 状态1的所有取值：
    for 状态2 in 状态2的所有取值：
        for ...
            dp[状态1][状态2][...] = 求最值(选择1，选择2...)
```

### 思考
**学习-练习-总结-归纳**

果然是非常重要、有效的一种学习方式，终生学习，不断努力！






