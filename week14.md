# ARTS Week 14
>[![Bw84pt.jpg](https://s1.ax1x.com/2020/11/01/Bw84pt.jpg)](https://imgchr.com/i/Bw84pt)
>> 如果不竭尽全力到最后一刻的话，是无法取胜的 ——朝田诗乃 《刀剑神域2》

***
## Algoithm
>[分隔链表](https://leetcode-cn.com/problems/partition-list)

### 概述
给定一个链表和一个特定值 x，对链表进行分隔，使得所有小于 x 的节点都在大于或等于 x 的节点之前。

你应当保留两个分区中每个节点的初始相对位置。
 

示例:

输入: head = 1->4->3->2->5->2, x = 3
输出: 1->2->2->4->3->5

### 分析
将链表分割成 左右两个链表，然后依次进行遍历，这里需要注意的就是一定要注意，有可能会成为**环**！

### 代码

```python

# Definition for singly-linked list.
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None


class Solution:
    def partition(self, head: ListNode, x: int) -> ListNode:
        # 分成两个链表，>x 链表同步 <=x 链表失败
        # 左边链表 小于 x
        left = ListNode(-1)
        left_start = left

        # 右边链表 大于等于x
        right = ListNode(-1)
        right_start = right

        while head:
            # 因为 中间会产生 环，所以会及时同步
            node = head.next

            if head.val < x:
                left.next = head
                left = left.next
            else:
                right.next = head
                right = right.next

            # 这里是为了断开 环
            head.next = None
            head = node

        # 左右链表拼接
        left.next = right_start.next
        return left_start.next

```

### 总结
链表问题，可以通过多个指针进行解析，同时也需要注意防止成为 **环**

***
## Review
>[](todo)

### 概述
todo


***
## Tip
>[故障排查与根因分析](https://time.geekbang.org/column/article/157416)
>[过载保护与容量规划](https://time.geekbang.org/column/article/159848)

### 概述
这部分主要是说明了分布式日志追踪+过载处理

#### 日志
日志追踪工具：[分布式跟踪的工具](https://opentracing.io/)

每次请求都生成 唯一 Request ID，能够快速追踪


排查故障的系统时间花费 大部分是 **“假设 - 验证排除”** 所以总结这些东西

1. 在错误的方向上浪费时间。关注了错误的系统现象，或者错误地理解了系统现象的含义。
2. 试图解决与当前系统问题相关的一些问题，却没有认识到这些其实只是巧合，或者这些问题其实是由于当前系统的问题造成的。比如，数据库压力大的情况下可能导致机房局部环境温度也有所上升，于是试图解决环境温度问题。
3. 将问题过早地归结为极不可能的因素，或者念念不忘之前曾经发生过的系统问题，认为还是由之前的问题造成。
4. 不正确地修改了系统的配置信息、输入信息或者系统运行环境，造成不能安全和有效地测试假设，导致验证的逻辑本身存在问题。


#### 过载
重试、重要性级别、请求延迟和截止时间、客户端侧的节流机制

##### 重试

1. 限制每个请求的重试次数，比如 2 次。
2. 不要将请求无限重试。一定要使用随机化的、指数型递增的重试周期。如果重试不是随机分布在重试窗口里的，那么系统出现的一个小故障，比如发生某个网络问题，就可能导致这些重试请求同时出现，进而引发过载。另外，如果请求没有成功，以指数型延迟重试。比如第一次是 3 秒后重试，那么第二次 6 秒，第三次 12 秒，以此类推。
3. 考虑使用一个全局重试预算。例如，每个进程每分钟只允许重试 60 次，如果重试预算耗尽，那么直接将这个请求标记为失败，而不真正发送它。这个策略可以在全局范围内限制住重试造成的影响，容量规划失败可能只是会造成某些请求被丢弃，而不会造成全局性的故障。

##### 重要性级别
1. 不同级别的权限设置

##### 请求延迟和截止时间
1. 设置请求超时时间

##### 客户端侧的节流机制
1. 请求数量（requests）：应用层代码发出的所有请求的数量总计。
2. 请求接受数量（accepts）：被服务端接受处理的请求数量。

***
## Share
>[因为 lock 导致的进程陷入假死状态](https://github.com/Carmenliukang/ARTS/blob/master/week14.md#share)

### 概述
**lock** ![java 锁机制与优化](https://blog.csdn.net/SumResort_LChaowei/article/details/72857921)

使用 celery crontab ，增加 lock & timeout ，但是中间有一个 sql 因为没有索引，执行过慢，导致 其他定时任务阻塞的情况。这种情况比较难以判断。


#### 表象
1. 进程存在，但是 CPU、MEN 都为0，其他定时任务显示阻塞状态。
2. 重启后初期显示正常，但是运行一段时间后，又再次出现。
3. 解除限制后，系统定时任务，一直再 开 thread 去执行，命令，导致系统无法再开 新的 thread，导致系统 进程 奔溃。
4. 后期因为 sql 执行的原因，导致 mysql database lock ，手动进入系统后 kill 相关的进程。
5. 到了 第4步，才发现这个是因为 sql 执行的问题。


#### 解决办法
1. mysql databases lock 后，快速链接数据库 kill 相关的进程。
2. 查看相关 sql ，然后找到 lock 的问题，project 中找到 相关sql。快速停止任务。
3. 查看 sql 问题，快速优化，增加索引，同时增加 sql 规则校验。

#### 反思
1. 这种情况比较难以发现，同时也是因为对 celery 的crontab 的底层机制的不了解，在 Redis 中查看其 lock 的key，然后再次解决。
2. 同时需要对相关任务的 整体流程进行查看，不能基金单独的追求在一个点，也需要快速的 走整个流程，然后一步一步走。
3. 相关的 sql 在小范围的时候没有问题，不代表大范围的数据也没有问题，这里还是需要尽可能的 规避这种 大范围数据有可能出现的问题。
4. update 的操作，如果有相关的 主键ID，可以通过多个 ORM 异步更新。

