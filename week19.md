# ARTS Week 19
>[![DoUaXq.jpg](https://s3.ax1x.com/2020/12/02/DoUaXq.jpg)](https://imgchr.com/i/DoUaXq)
>> 静水流深，沧笙踏歌

***
## Algoithm
>[首个共同的祖先](https://leetcode-cn.com/problems/first-common-ancestor-lcci)

### 概述
设计并实现一个算法，找出二叉树中某两个节点的第一个共同祖先。不得将其他的节点存储在另外的数据结构中。注意：这不一定是二叉搜索树。

    例如，给定如下二叉树: root = [3,5,1,6,2,0,8,null,null,7,4]
    
        3
       / \
      5   1
     / \ / \
    6  2 0  8
      / \
     7   4
    示例 1:
    
    输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
    输出: 3
    解释: 节点 5 和节点 1 的最近公共祖先是节点 3。
    示例 2:
    
    输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
    输出: 5
    解释: 节点 5 和节点 4 的最近公共祖先是节点 5。因为根据定义最近公共祖先节点可以为节点本身。
    说明:
    
    所有节点的值都是唯一的。
    p、q 为不同节点且均存在于给定的二叉树中。

### 思考
1. 每一个节点都是唯一
2. p&q 一定都在树中

分成三种情况：
1. p/q 分别在左右子树中，那么最近的节点就是 root节点
2. p&q 都在同一子树中，那么就返回其子树的 root节点
3. p/q 其中一个为最近的节点，那么直接返回其节点

然后依次进行遍历
***

### coding

```python

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None


class Solution:
    """
    1. 题目中说明 节点都是 唯一
    2. p & q 都在树中

    按照不同情况区分：
    1. p/q 在root的左右子树中
    2. p&q 在root的同一个子树中
    3. p/q 为其中一个跟节点

    依次按照这种情况进行递归查询

    """
    def lowestCommonAncestor(self, root: TreeNode, p: TreeNode, q: TreeNode) -> TreeNode:
        if not root or root == p or root == q:
            return root

        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)
        if not left:
            return right

        if not right:
            return left

        if left and right:
            return root

```

### 总结
1. 树的问题大部分都可以通过递归来进行解决

递归的方法论：
1. 终止条件：
    1. not root
    2. left==right 
    3. root==p
2. 递归问题：
    1. 将问题进行分解，分成不同的情况进行沟通处理。
3. 递归顺序：
    1. 前序
    2. 中序
    3. 后序

## Review
>[XGBoost Algorithm: Long May She Reign!](https://towardsdatascience.com/https-medium-com-vishalmorde-xgboost-algorithm-long-she-may-rein-edd9f99be63d)

### 概述
>[github xgboost](https://github.com/dmlc/xgboost/)

>![DxLw34.jpg](https://s3.ax1x.com/2020/12/07/DxLw34.jpg)


**XGBoost** is a decision-tree-based ensemble Machine Learning algorithm that uses a [gradient boosting](https://en.wikipedia.org/wiki/Gradient_boosting) framework.

1. A wide range of applications: Can be used to solve regression, classification, ranking, and user-defined prediction problems.
2. Portability: Runs smoothly on Windows, Linux, and OS X.
3. Languages: Supports all major programming languages including C++, Python, R, Java, Scala, and Julia.
4. Cloud Integration: Supports AWS, Azure, and Yarn clusters and works well with Flink, Spark, and other ecosystems.

#### XGBoots 
1. Decision Tree: Every hiring manager has a set of criteria such as education level, number of years of experience, interview performance. A decision tree is analogous to a hiring manager interviewing candidates based on his or her own criteria.
2. Bagging: Now imagine instead of a single interviewer, now there is an interview panel where each interviewer has a vote. Bagging or bootstrap aggregating involves combining inputs from all interviewers for the final decision through a democratic voting process.
3. Random Forest: It is a bagging-based algorithm with a key difference wherein only a subset of features is selected at random. In other words, every interviewer will only test the interviewee on certain randomly selected qualifications (e.g. a technical interview for testing programming skills and a behavioral interview for evaluating non-technical skills).
4. Boosting: This is an alternative approach where each interviewer alters the evaluation criteria based on feedback from the previous interviewer. This ‘boosts’ the efficiency of the interview process by deploying a more dynamic evaluation process.
5. Gradient Boosting: A special case of boosting where errors are minimized by gradient descent algorithm e.g. the strategy consulting firms leverage by using case interviews to weed out less qualified candidates.
6. XGBoost: Think of XGBoost as gradient boosting on ‘steroids’ (well it is called ‘Extreme Gradient Boosting’ for a reason!). It is a perfect combination of software and hardware optimization techniques to yield superior results using less computing resources in the shortest amount of time.

***
## Tip
>[架构老化与重构](https://time.geekbang.org/column/article/180396)

### 概述
1. 如何重构代码
    1. 尽可能避免核心代码修改
    2. 新业务独立，避免核心代码侵入
2. 其他方面
    1. 实际上从难度来说，重构比一个全新业务的架构过程要难得多。
    2. 重构，不只是一个架构的合理性问题。它包含了架构合理性的考量，因为我们需要知道未来在哪里，我们迭代方向在哪里。
    3. 但重构的挑战远不只是这些。这是一个集架构设计（未来架构应该是什么样的）、资源规划与调度（与新功能开发的优先级怎么排）、阶段规划（如何把大任务变小，降低内部的抵触情绪和项目风险）以及持久战的韧性与毅力的庞大工程。
3. 我们不能指望有一天架构水平会突飞猛进。架构能力提升全靠平常一点一滴地不断反思与打磨得来

### 引用 leslie 

引用老师课程中关于重构的一句经典话语"架构设计（未来架构应该是什么样的）、资源规划与调度（与新功能开发的优先级怎么排）、阶段规划（如何把大任务变小，降低内部的抵触情绪和项目风险）以及持久战的韧性与毅力的庞大工程。”体现了老师一直强调的架构与业务的理解。
软件架构的老化与重构参与不多：不过数据库架构这块的事情经历过不少。虽范围有所缩小，不过核心思路大致相同。对于老师的这个总结拆分简析一下；
#### 架构设计
1. 需要梳理出当前的现状，对于整体现状做出分析；目的是再烂的架构都有其合理性，其中那些可能会被将来做为最小原子使用这是需要做的；
2. 针对分析的结果再权衡利弊的基础上想出改进方案，毕竟重构升级的过程还是有许多关联性的数据；
3. 未来的短、中、长期规划大致是怎样，怎样才能可扩展或后期升级。
#### 资源规划与调度
1. 资源规划：要做的就是拆分，需要对于团队/项目有足够的了解才能更好的明白和了解有什么样资源以及可以用到什么样的程度
2. 资源调度：任何一个项目会有固定资源和非固定/调用资源，固定和非固定的使用程度和时间完全不同的且了解不同，这个协调能力是一个项目经理所需的能力。
#### 阶段规划以及持久性
1. 格局观和可持续性，即通常所说的CI/CD特性；对于整体的了解越明白、格局观与弹性越好，规划和持续性就越好。
2. 其实还涉及到产品中常用的MVP特性，试错中找到最佳持续方案。


以上是我对于老师今天分享的思考和理解以及梳理；一路学习、一路实践、一路反思、一路收获。感谢老师的付出，让我在学习中能不断收获到不一样的知识；期待老师的后续分享，谢谢。


***
## Share
>[树类型题目总结](https://github.com/Carmenliukang/ARTS/blob/master/week19.md#share)

### 概述
总结相关的类型题目

#### 双层DFS
>[二叉树的完全性验证](https://leetcode-cn.com/problems/check-completeness-of-a-binary-tree)
>[二叉树的最大宽度](https://leetcode-cn.com/problems/maximum-width-of-binary-tree)
>[检查子树](https://leetcode-cn.com/problems/check-subtree-lcci)

##### 二叉树的完全性验证
>[二叉树的完全性验证](https://leetcode-cn.com/problems/check-completeness-of-a-binary-tree)

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None


class Solution:
    def isCompleteTree(self, root: TreeNode) -> bool:
        nodes = [(root, 1)]
        i = 0
        # 使用二叉树的层序遍历进行相关节点的校验
        while i < len(nodes):
            node, v = nodes[i]
            i += 1
            if node:
                nodes.append((node.left, 2 * v))
                nodes.append((node.right, 2 * v + 1))

        return nodes[-1][1] == len(nodes)
```

##### 二叉树的最大宽度
```python

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:
    def __init__(self):
        self.ans = 0
        self.left = {}

    def widthOfBinaryTree(self, root: TreeNode) -> int:
        self.dfs(root)
        return self.ans

    def dfs(self, root, depth=0, pos=0):
        if not root:
            return

        # 记录每一层的最左边的数据大小，然后将其
        self.left.setdefault(depth, pos)
        self.ans = max(self.ans, pos - self.left[depth] + 1)
        self.dfs(root.left, depth + 1, 2 * pos)
        self.dfs(root.right, depth + 1, 2 * pos + 1)
```

##### 相同的树
>[相同的树](https://leetcode-cn.com/problems/same-tree)
```python

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:
    def isSameTree(self, p: TreeNode, q: TreeNode) -> bool:
        if not p and not q:
            return True

        if not p or not q:
            return False
        # 这里使用的是 前序遍历，因为是相同的树，所以 是相同的 左右子树，如果是对称，那么就需要同步其他的状态了。
        return p.val == q.val and self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)


```

##### 检查子树
>[检查子树](https://leetcode-cn.com/problems/check-subtree-lcci)

```python

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None


class Solution:
    """
    判断t2是否为t1的子树，如果根节点相同，那么有必要开始判断从这个根节点开始这两棵树是否相同。若相同了，说明已经找到，结束算法；如果不同，继续从t1的左右子树寻找t2，只要在一边找到就可以了。
    根节点相同但仍然是不同的树，如 t1 = [2,2,3], t2 = [2]

    如果根节点不相同，那么继续从t1的左右子树寻找t2，只要在一边找到就可以了

    递归出口：空树认为是任何树的子树；当t1为空而t2不为空时，说明t1不包含t2。

    作者：zui-weng-jiu-xian
    链接：https://leetcode-cn.com/problems/check-subtree-lcci/solution/jian-cha-zi-shu-jin-100dai-ma-jian-ji-zhu-shi-xian/
    来源：力扣（LeetCode）
    著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

    """

    def checkSubTree(self, t1: TreeNode, t2: TreeNode) -> bool:
        if not t2:
            return True
        if not t1:
            return False
        # 两层 dfs 查询，然后同步
        return self._dfs(t1, t2) or self.checkSubTree(t1.left, t2) or self.checkSubTree(t1.right, t2)

    def _dfs(self, t1, t2):
        # 判断其两个树是否相同
        if not t2:
            return True
        if not t1:
            return False

        if t1.val != t2.val:
            return False

        return self._dfs(t1.left, t2.left) and self._dfs(t1.right, t2.right)

```

##### 总结
可以将每一个数值都设置成可固化的数值，然后进行相关的计算

