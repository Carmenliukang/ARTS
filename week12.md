# ARTS Week 12
>![](https://image.uc.cn/s/wemedia/s/upload/2020/c3aad88339c5233577627dfb7844e0a8.png)
>> 坚强不是结果，是朝某个目标努力的过程！


***
## Algoithm
> [奇偶链表](https://leetcode-cn.com/problems/odd-even-linked-list)

### 概述
给你一棵以 root 为根的二叉树和一个 head 为第一个节点的链表。

如果在二叉树中，存在一条一直向下的路径，且每个点的数值恰好一一对应以 head 为首的链表中每个节点的值，那么请你返回 True ，否则返回 False 。

一直向下的路径的意思是：从树中某个节点开始，一直连续向下的路径。


示例 1：

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/29/sample_1_1720.png)

输入：
    
    head = [4,2,8], 
    
    root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]

输出：true

解释：树中蓝色的节点构成了与链表对应的子路径。


示例 2：

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/29/sample_2_1720.png)


输入：

    head = [1,4,2,6], 
       
    root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]

输出：true

示例 3：

输入：
    
    head = [1,4,2,6,8], 
    
    root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]

输出：false

解释：二叉树中不存在一一对应链表的路径。



来源：力扣（LeetCode）

链接：https://leetcode-cn.com/problems/linked-list-in-binary-tree

著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。


### 分析
方法一：枚举

枚举二叉树中的每个节点为起点往下的路径是否有与链表相匹配的路径。为了判断是否匹配我们设计一个递归函数 dfs(rt,\textit{head})dfs(rt,head) ，其中 rtrt 表示当前匹配到的二叉树节点，headhead 表示当前匹配到的链表节点，整个函数返回布尔值表示是否有一条该节点往下的路径与 headhead 节点开始的链表匹配，如匹配返回 true，否则返回 false ，一共有四种情况：

链表已经全部匹配完，匹配成功，返回 true

二叉树访问到了空节点，匹配失败，返回 false

当前匹配的二叉树上节点的值与链表节点的值不相等，匹配失败，返回 false

前三种情况都不满足，说明匹配成功了一部分，我们需要继续递归匹配，所以先调用函数 dfs(rt→left,head→next) ，其中 leftrt→left 表示该节点的左儿子节点，nexthead→next 表示下一个链表节点，如果返回的结果是 false，说明没有找到相匹配的路径，需要继续在右子树中匹配，继续递归调用函数 dfs(rt→right,head→next) 去找是否有相匹配的路径，其中 rightrt→right 表示该节点的右儿子节点， head→next 表示下一个链表节点。

匹配函数确定了，剩下只要枚举即可，从根节点开始，如果当前节点匹配成功就直接返回 true ，否则继续找它的左儿子和右儿子是否满足，也就是代码中的 dfs(root,head) || isSubPath(head,root->left) || isSubPath(head,root->right) ，然后不断的递归调用。

这样枚举所有节点去判断即能找出是否有一条与链表相匹配的路径。


### 代码

```python

# Definition for singly-linked list.
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None


# Definition for a binary tree node.
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None


class Solution:

    def dfs(self, head: ListNode, root: TreeNode) -> bool:
        if not head:
            return True

        if not root:
            return False

        if head.val != root.val:
            return False

        return self.dfs(head.next, root.right) or self.dfs(head.next, root.left)

    def isSubPath(self, head: ListNode, root: TreeNode) -> bool:
        if not root:
            return False

        return self.dfs(head, root) or self.isSubPath(head, root.left) or self.isSubPath(head, root.right)

```


### 总结
链表的问题都可以通过画图的形式展示出来，然后一步一步运行。


***
## Review
> [TODO](TODO)

### 概述
TODO

***
## Tip
TODO
> [服务端的业务架构建议](https://time.geekbang.org/column/article/134384)

### 概述
服务端整体可以分为以下几个模块：
1. 网络协议
2. 授权
3. RPC框架
4. 单元测试


#### 网络协议
1. HTTP

无状态的 文本传输协议

2. thrift

FB 提供的基于 HTTP 协议的 RPC 框架调用方式

3. protobuf grpc

基于HTTP 2.0 协议，使用字节流方式加快数据传输。

字节流方式对标的是 json/xml/form 等这种文件传输格式


4. GraphQL

[为什么没有火起来？](https://www.zhihu.com/question/38596306)

#### 授权

脱离业务

1. Token

2. AK/SK

#### RPC 框架

1. thrift
2. grpc


#### 单元测试
1. mock 方式测试

***
## Share
>[服务治理的宏观视角](https://time.geekbang.org/column/article/144803)

### 概述
服务治理 

实现了从 硬件 >>> 虚拟 >>> 自动 >>> 自治  的变化

能够快速的找打问题发现的原因，需要在日志、监控、异常信息手机 等快速通过和联系。

