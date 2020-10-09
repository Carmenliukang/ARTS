# ARTS Week 9
>![](https://s1.ax1x.com/2020/07/08/UVm98U.jpg)
>> 凡是过往，皆为序章


***
## Algoithm
> [旋转链表](https://leetcode-cn.com/problems/rotate-list)

### 概述
给定一个链表，旋转链表，将链表每个节点向右移动 k 个位置，其中 k 是非负数。

示例 1:

输入: 1->2->3->4->5->NULL, k = 2
输出: 4->5->1->2->3->NULL
解释:
向右旋转 1 步: 5->1->2->3->4->NULL
向右旋转 2 步: 4->5->1->2->3->NULL
示例 2:

输入: 0->1->2->NULL, k = 4
输出: 2->0->1->NULL
解释:
向右旋转 1 步: 2->0->1->NULL
向右旋转 2 步: 1->2->0->NULL
向右旋转 3 步: 0->1->2->NULL
向右旋转 4 步: 2->0->1->NULL


### 分析

先获取其链表深度，进行取余，然后list 切割

### 代码

```python

# Definition for singly-linked list.
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None


class Solution:
    def rotateRight(self, head: ListNode, k: int) -> ListNode:
        """
        1. 获取深度
        2. 计算 index
        3. index 节点 设置为 头结点， 链表最后一个节点对应开头节点，index-1 设置为最后一个节点
        """

        # 判断其最后的结果是否一致
        if not head or not k:
            return head

        nums = []
        total = 0

        # 这里需要注意的是需要同步其他的节点
        p = head
        while p:
            total += 1
            nums.append(p.val)
            p = p.next

        # 有可能需要循环，所以，记性取余操作。
        index = k % total
        if not index:
            return head

        # 获取最后的链表结果
        res = nums[-index:] + nums[:total - index]

        # 生成相关的状态截图
        root = ListNode(-1)
        p = root
        for i in res:
            node = ListNode(i)
            root.next = node
            root = root.next

        return p.next

```


### 总结
单向链表结构，可以先遍历，获取结果，然后再进行同步。

```
res = []
while head:
    total += 1
    res.append(head.val)
    head = head.next
```

***
## Review
> [Setting up HTTPS/TLS between a Kubernetes cluster and an iOS device with a self-signed certificate](https://medium.com/@nintendoengineer12/setting-up-https-tls-between-a-kubernetes-cluster-and-an-ios-device-with-a-self-signed-certificate-61eeed56b66d)

### 概述
1. 介绍自有签名设置为安全可信任的方法。



***
## Tip
> [redis 基础架构](https://time.geekbang.org/column/article/268262)

### 概述


1. 访问框架
    
    socket server 
    支持了更加多的通用架构
    
2. 操作模块
    
   
3. 索引模块

4. 存储模块
    AOF & RDB


![](https://s1.ax1x.com/2020/10/09/0rU9UA.jpg)
### 问题与思考
1. 整数数组和压缩列表在查找时间复杂度方面并没有很大的优势，那为什么 Redis 还会把它们作为底层数据结构呢？
    
    1. 内存利用率，数组和压缩列表都是非常紧凑的数据结构，它比链表占用的内存要更少。Redis是内存数据库，大量数据存到内存中，此时需要做尽可能的优化，提高内存的利用率。
    
    2. 数组对CPU高速缓存支持更友好，所以Redis在设计时，集合数据元素较少情况下，默认采用内存紧凑排列的方式存储，同时利用CPU高速缓存不会降低访问速度。当数据元素超过设定阈值后，避免查询时间复杂度太高，转为哈希和跳表数据结构存储，保证查询效率。


***
## Share
>[AOF 原理](https://time.geekbang.org/column/article/271754)

### 概述
1. 执行命令后才会写入到日志中，不会影响当前语句执行，但是有可能会阻塞后面执行的语句，
2. 仅仅记录语句，不会影响其他的语句
3. 重写机制，合并相同操作，降低 文件大小。异步重写进程同步。
![](https://s1.ax1x.com/2020/10/09/0rdP6f.jpg)

### 思考
没有完全匹配的结果，只有平衡策略的一致性
