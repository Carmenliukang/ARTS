# ARTS Week 17
>[![DaWz6J.jpg](https://s3.ax1x.com/2020/11/25/DaWz6J.jpg)](https://imgchr.com/i/DaWz6J)
>> 人根本沒辦法在不喜歡的事物上努力啊。

***
## Algoithm
>[二叉树的右视图](https://leetcode-cn.com/problems/binary-tree-right-side-view)

### 概述
```
给定一棵二叉树，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

示例:

输入: [1,2,3,null,5,null,4]
输出: [1, 3, 4]
解释:

   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---
```

### 分析
1. 先递归获取其深度。
2. 同步状态修改。


### codeing
```python

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None


class Solution(object):
    def rightSideView(self, root):
        # 使用栈的方式进行数据的整体的遍历，实现了层序的遍历
        rightmost_value_at_depth = dict()  # 深度为索引，存放节点的值
        max_depth = -1

        stack = [(root, 0)]
        while stack:
            node, depth = stack.pop()

            if node is not None:
                # 维护二叉树的最大深度
                max_depth = max(max_depth, depth)

                # 如果不存在对应深度的节点我们才插入
                rightmost_value_at_depth.setdefault(depth, node.val)

                stack.append((node.left, depth + 1))
                stack.append((node.right, depth + 1))

        return [rightmost_value_at_depth[depth] for depth in range(max_depth + 1)]


```

***
## Review
>[Microservices Design - API Gateway Pattern](https://medium.com/dev-genius/microservices-design-api-gateway-pattern-980e8d02bdd5)

### 概述
“Microservice is a tightly scoped, strongly encapsulated, loosely coupled, independently deployable, and independently scalable application component.”

* Each microservice can be deployed, upgraded, scaled, maintained, and restarted independent of sibling services in the application.
* Agile development & agile deployment with an autonomous cross-functional team.
* Flexibility in using technologies and scalability.

[![DrSifP.png](https://s3.ax1x.com/2020/11/27/DrSifP.png)](https://imgchr.com/i/DrSifP)

### API Gateway
[![Dr99RP.png](https://s3.ax1x.com/2020/11/27/Dr99RP.png)](https://imgchr.com/i/Dr99RP)


***
## Tip
>[实战：“画图程序” 的整体架构](https://time.geekbang.org/column/article/172004)
>[全局性功能的架构设计](https://time.geekbang.org/column/article/173619)

### 概述

#### 系统侵入计算方式
[画图代码](https://github.com/qiniu/qpaint/tree/v44)

计算方式：

比如某个接口方法被 N 个周边模块引用，那么每个周边模块分担的伤害值为 1/N。


#### 全局性功能设计
**任何功能都是可以正交分解的，即使我目前还没有找到方法。**

**业务分解就是最小化的核心系统，加上多个正交分解的周边系统。核心系统一定要最小化，要稳定。坚持不要往核心系统中增加新功能，这样你的业务架构就不可能有臭味**


***
## Share
>[todo]()

### 概述
todo  