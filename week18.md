# ARTS Week 18
>[![DryJYV.jpg](https://s3.ax1x.com/2020/11/27/DryJYV.jpg)](https://imgchr.com/i/DryJYV)
>> 他死在一个谁都不知道角落我不想他的骨灰毫无意义。

***
## Algoithm
>[找树左下角的值](https://leetcode-cn.com/problems/find-bottom-left-tree-value)

### 概述
    给定一个二叉树，在树的最后一行找到最左边的值。
    
    示例 1:
    
    输入:
    
        2
       / \
      1   3
    
    输出:
    1
     
    
    示例 2:
    
    输入:
    
            1
           / \
          2   3
         /   / \
        4   5   6
           /
          7
    
    输出:
    7
 

注意: 您可以假设树（即给定的根节点）不为 NULL。

### 分析
这里是可以通过前序遍历，记录每一层的最左边节点的数据，然后同步结果。


### codeing

```python

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None


class Solution:
    def __init__(self):
        self.res = []

    def findBottomLeftValue(self, root: TreeNode) -> int:
        self.dfs(root, 0)
        return None if not self.res else self.res[-1]

    def dfs(self, node, depth):
        # 使用前序 记录每一层的数据
        if not node:
            return

        if depth == len(self.res):
            self.res.append(node.val)

        depth += 1
        self.dfs(node.left, depth)
        self.dfs(node.right, depth)

    def findBottomLeftValueMethod(self, root: TreeNode) -> int:
        # 使用栈的方式同步其每一层的数据，同时记录其相关的结果
        queue = [root]
        while queue:
            node = queue.pop(0)
            if node.right:  # 先右后左
                queue.append(node.right)
            if node.left:
                queue.append(node.left)
        return node.val

```

***
## Review
>[Binary Tree - Interview Questions and Practice Problems](https://medium.com/techie-delight/binary-tree-interview-questions-and-practice-problems-439df7e5ea1f)
>[binary_tree](https://en.wikipedia.org/wiki/Binary_tree)
>>[![DL25TK.jpg](https://s3.ax1x.com/2020/12/05/DL25TK.jpg)](https://imgchr.com/i/DL25TK)

### 概述
 
A **Binary Tree** is a tree data structure in which each node has at most two children, which are referred to as the left child and the right child and the topmost node in the tree is called the root.

[![DLRilj.png](https://s3.ax1x.com/2020/12/05/DLRilj.png)](https://imgchr.com/i/DLRilj)

列举了相关的题目
>[Check if two given binary trees are identical or not](https://www.techiedelight.com/check-if-two-binary-trees-are-identical-not-iterative-recursive/)

可以查看leetcode算法tree tag
>[leetcode-tree](https://leetcode-cn.com/tag/tree/)


***
## Tip
>[重新认识开闭原则 (OCP)](https://time.geekbang.org/column/article/175236)

### 概述
**架构的本质是业务的正交分解。**

架构的两大难题：
1. 需求的交织
2. 需求的易变

架构的工具：
1. 组合
2. 开闭原则

### 开闭原则（OCP）
> 软件实体（模块，类，函数等）应该对于功能扩展是开放的，但对于修改是封闭的。

架构从 **"只读"** 思考未来

### CPU 背后的架构思维
> 从需求分析角度来说，关键要抓住需求的稳定点和变化点。需求的稳定点，往往是系统的核心价值点；而需求的变化点，则往往需要相应去做开放性设计。

冯·诺依曼体系的中央处理器（CPU）的设计完美体现了 “开闭原则” 的架构思想。它表现在：
1. 指令是稳定的，但指令序列是变化的，只有这样计算机才能够实现 “解决一切可以用 ‘计算’ 来解决的问题” 这个目标。
2. 计算是稳定的，但数据交换是多变的，只有这样才能够让计算机不必修改基础架构却可以适应不断发展变化的交互技术革命。

### 插件机制
1. 支持插件系统
2. 插件的通用型

### 单一职责原则
1. 一个模块只修改其中一个模块
2. 如果需要修改核心逻辑，那么可以将其重新开辟新的业务

***
## Share
>[知乎上刷题的感悟](https://www.zhihu.com/question/338247553/answer/773524828)

### 概述
刷题现在遇到了一个瓶颈，中等难度的题目有一些，总是感觉会，但是写不出来，反思了一下，其实还是对问题的抽象能力太低了。这里还是需要快速提升才可以。

### 问题
智商太低学不会算法，还能搞CS吗？

1. 被天灭伪化deeducated了4年，别说算法导论，数据结构和算法分析都看不下去，是不是只能zs，或者当一辈子调参侠了？（绝望）（

### [高赞回答](https://www.zhihu.com/question/338247553/answer/773524828)
这个问题本质上其实不是算法的问题，而是心理问题。从描述看来，这是妄自菲薄，典型的自挫性思维。
1. **思维扭曲一**:以偏概全，以特殊推一般。“算法学习遇到挫折”-->“我失败了，人生从此绝望”-->将自我全盘否定。“被天灭伪deeducated了4年”，非黑即白，非此即彼，认为自己从未学到过知识，觉得知识不是好的就是坏的，认为自己4年受到的教育没有丝毫好的，里面没有任何好的东西。
2. **思维扭曲二**:情绪化推理。把心理感受当成事实，感觉自己“智商”很低，所以认为自己肯定就是了。
3. **思维扭曲三**:自我贴标签。给自己贴上“天生智商低，不适合学习”的标签，而不是以解决问题为核心，思考面对挫折有哪些可能应对途径。把自己当前工作概括贴标签，定义自己是“调参侠”，体现出对所作工作的低劣评价，认为所做之事毫无意义，是“lowB”才会做的“毫无技术含量”的工作。
4. **思维扭曲四**:认为自己无所不能，是神。“别说算法导论”说明自己内心是对算法导论这本书蔑视和不屑一顾的，内心想当然的认为“这么简单的书，我可以轻轻松松刷完”。认为自己学习算法应该永远顺风顺水，绝对不能遇到任何不懂，不能遇到任何困难，否则自己就是智商低。然而世事并非总如人所愿，“数据结构和算法分析都看不下去”，幻想被打破，然后对自己感到挫败，不再无所不能，不是神了，不再是“成功人士”家族了，自己就是失败者，就只能zs了，就只能在浅色床单上哭泣。

这个问题归根结底不是算法的问题，而是题主陷入了失败学的深渊，拥有扭曲的认知，被自挫性思维蒙蔽。首先面对和处理心理上的问题，再去学习算法，自然能解决。


### 思考
其实很多的时候，都存在这样的情况，妄自菲薄，没有能够直面困难，接受自己的平凡。看完这句话心态就变好了很多。
1. 世界不是非黑即白，世界是非常复杂的，也是残酷的，有着绝对的善良，也有绝对的邪恶。
2. 把人生当成一种修行。