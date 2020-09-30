# ARTS Week 8
>![](https://s1.ax1x.com/2020/07/08/UVm98U.jpg)
>> 展望未来，温暖常在


***
## Algoithm
> [回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)

### 概述
请判断一个链表是否为回文链表。

示例 1:

输入: 1->2
输出: false
示例 2:

输入: 1->2->2->1
输出: true


### 分析

通用方法，可以使用数组存储结果，然后依次进行对照遍历

### 代码

```python3
# Definition for singly-linked list.
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None

class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        res = [] # 存储链表的遍历结果
        total = 0 # 记录深度
        
        # 遍历获取每一个节点的 val 数值
        while head:
            total += 1
            res.append(head.val)
            head = head.next
        
        # 节省遍历次数，提高性能
        for i in range(total//2):
            if res[i] != res[-i-1]:
                return False
        
        return True

```


### 总结
单向链表结构，一般情况的 遍历方式：

```
res = []
while head:
    total += 1
    res.append(head.val)
    head = head.next
```

然后进行相关的计算方式尝试


***
## Review
> [HTTP 3](https://towardsdatascience.com/http-3-is-out-and-about-7c903f9aab9e)

### 概述
1. HTTP 流程
    ![](https://s1.ax1x.com/2020/09/30/0noEIU.png)

2. http2
    1. 支持多路复用
    2. 服务器推送

3. http3
    1. TCP 三次握手优化，使用[QUIC协议](https://medium.com/@anuradhawick/the-quic-internet-its-the-future-d903440b26ea)
    2. 增强了移动迁移，WiFi链接与移动网络链接等场景中。

### 思考
文章初步介绍了相关框架，对于内部实现的原理、机制等没有进行深入了解，可以作为一些前瞻性的补充。

***
## Tip
> [redis 哪些慢操作](https://time.geekbang.org/column/article/268253)

### 概述
1. 哈希桶 
    
    全局哈希+哈希桶 为了提高系统对于内存的使用率。
    ![](https://s1.ax1x.com/2020/09/30/0n7ACF.jpg)
2. rehash
    
    为了提高对于系统内存的使用率，降低哈希桶的深度，所以会渐进式 rehash
    ![](https://s1.ax1x.com/2020/09/30/0n7l4O.jpg)
   
3. 底层存储结构
    
    ![](https://s1.ax1x.com/2020/09/30/0nHKMj.jpg)

### 问题与思考
1. 整数数组和压缩列表在查找时间复杂度方面并没有很大的优势，那为什么 Redis 还会把它们作为底层数据结构呢？
    
    1. 内存利用率，数组和压缩列表都是非常紧凑的数据结构，它比链表占用的内存要更少。Redis是内存数据库，大量数据存到内存中，此时需要做尽可能的优化，提高内存的利用率。
    
    2. 数组对CPU高速缓存支持更友好，所以Redis在设计时，集合数据元素较少情况下，默认采用内存紧凑排列的方式存储，同时利用CPU高速缓存不会降低访问速度。当数据元素超过设定阈值后，避免查询时间复杂度太高，转为哈希和跳表数据结构存储，保证查询效率。


***
## Share
>[高性能IO模型](https://time.geekbang.org/column/article/270474)

### 概述
1. 多路复用
2. 单线程模型
    1. 多线程 共享资源 并发访问控制等
        ![](https://s1.ax1x.com/2020/09/30/0nHWyd.jpg)

### 思考
异步非阻塞方式是一种趋势
