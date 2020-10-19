# ARTS Week 11
>![](http://image.photocnc.com/photocnc/2020-04/15/202004151046469516.Jpeg.w1680.jpg)
>> 桐人现在正要把自己所说的话付诸行动，能够做到这一点，并不是因为桐人很强，而是他愿意接受自己的懦弱，烦恼、痛苦但即便如此依旧继续向前。


***
## Algoithm
> [奇偶链表](https://leetcode-cn.com/problems/odd-even-linked-list)

### 概述
给定一个单链表，把所有的奇数节点和偶数节点分别排在一起。请注意，这里的奇数节点和偶数节点指的是节点编号的奇偶性，而不是节点的值的奇偶性。

请尝试使用原地算法完成。你的算法的空间复杂度应为 O(1)，时间复杂度应为 O(nodes)，nodes 为节点总数。

示例 1:

输入: 1->2->3->4->5->NULL
输出: 1->3->5->2->4->NULL
示例 2:

输入: 2->1->3->5->6->4->7->NULL
输出: 2->3->6->7->1->5->4->NULL
说明:

应当保持奇数节点和偶数节点的相对顺序。
链表的第一个节点视为奇数节点，第二个节点视为偶数节点，以此类推。


### 分析
1. 通过位置，而不是通过里面的数值判断。
2. 画出来
![](https://pic.leetcode-cn.com/00bd1d974b5a2e6d7d4faf0d5baad1c691f4ed8963cb1b7133d1112bad4c5e86-image.png)
### 代码

```python

# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next


class Solution:
    def oddEvenList(self, head: ListNode) -> ListNode:
        if not head:
            return head
        # 使用指针进行同步，注意，这里是通过位置判断是否为奇偶，而不是通过判断里面的位置来进行判断。
        odd = head
        even = head.next
        even_start = even

        while even and even.next:
            odd.next = even.next
            odd = odd.next

            even.next = odd.next
            even = even.next

        odd.next = even_start
        return head

```


### 总结
链表的问题都可以通过画图的形式展示出来，然后通过多指针，或者递归的方式同步


***
## Review
> [redis RDB](https://time.geekbang.org/column/article/271839)

### 概述
TODO

***
## Tip
TODO
> [redis RDB](https://time.geekbang.org/column/article/271839)
> [redis 数据同步，主从一致](https://time.geekbang.org/column/article/272852)

### 概述

1. 高可用：
    1. 数据尽量减少丢失
    2. 服务尽量减少中断

2. 读写分离:
![](https://static001.geekbang.org/resource/image/80/2f/809d6707404731f7e493b832aa573a2f.jpg)

3. 同步过程:
![](https://static001.geekbang.org/resource/image/63/a1/63d18fd41efc9635e7e9105ce1c33da1.jpg)

4. 常用架构:
![](https://static001.geekbang.org/resource/image/40/45/403c2ab725dca8d44439f8994959af45.jpg)


### 思考

引用大神 Kaito 的回答：

2核CPU、4GB内存、500G磁盘，Redis实例占用2GB，写读比例为8:2，此时做RDB持久化，产生的风险主要在于 CPU资源 和 内存资源 这2方面：

a、内存资源风险：Redis fork子进程做RDB持久化，由于写的比例为80%，那么在持久化过程中，“写实复制”会重新分配整个实例80%的内存副本，大约需要重新分配1.6GB内存空间，这样整个系统的内存使用接近饱和，如果此时父进程又有大量新key写入，很快机器内存就会被吃光，如果机器开启了Swap机制，那么Redis会有一部分数据被换到磁盘上，当Redis访问这部分在磁盘上的数据时，性能会急剧下降，已经达不到高性能的标准（可以理解为武功被废）。如果机器没有开启Swap，会直接触发OOM，父子进程会面临被系统kill掉的风险。

b、CPU资源风险：虽然子进程在做RDB持久化，但生成RDB快照过程会消耗大量的CPU资源，虽然Redis处理处理请求是单线程的，但Redis Server还有其他线程在后台工作，例如AOF每秒刷盘、异步关闭文件描述符这些操作。由于机器只有2核CPU，这也就意味着父进程占用了超过一半的CPU资源，此时子进程做RDB持久化，可能会产生CPU竞争，导致的结果就是父进程处理请求延迟增大，子进程生成RDB快照的时间也会变长，整个Redis Server性能下降。

c、另外，可以再延伸一下，老师的问题没有提到Redis进程是否绑定了CPU，如果绑定了CPU，那么子进程会继承父进程的CPU亲和性属性，子进程必然会与父进程争夺同一个CPU资源，整个Redis Server的性能必然会受到影响！所以如果Redis需要开启定时RDB和AOF重写，进程一定不要绑定CPU。


![](https://s1.ax1x.com/2020/10/19/0vb2Yn.png)
***
## Share
TODO
>[linux fork](https://zhuanlan.zhihu.com/p/36872365)

### 概述
TODO

