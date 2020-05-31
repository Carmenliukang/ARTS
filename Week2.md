# ARTS week 1
> 凡是过往，即为序章

***

## Algoithm
> [打家劫舍](https://leetcode-cn.com/problems/house-robber/)

### 概述

    你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。
    
    给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。
    
    示例 1:
    
        输入: [1,2,3,1]
        输出: 4
        解释: 偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
             偷窃到的最高金额 = 1 + 3 = 4 。
    
    示例 2:
    
        输入: [2,7,9,3,1]
        输出: 12
        解释: 偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
                 偷窃到的最高金额 = 2 + 9 + 1 = 12 。


### 分析
    * 均为非负整数，所以尽可能的多，结果不变/越来越大。`如果为0，那么就垦呢个不变`
    * 为空数组，return 0
    * 如果为1，那么就直接返回
    * 如果多个，从0-N，那么就是 N-1 / N-2 是最大 total 

### 代码
```python
class Solution:
    def rob(self, nums: list[int]) -> int:
        # 首先判断其是否为 空数组
        if not nums:
            return 0
        
        # 获取数组长度
        size = len(nums)
        if size == 1: # 为1进行特殊逻辑处理
            return nums[0]
        
        # 开始获取开始最大的两个
        first, second = nums[0], max(nums[0], nums[1])
        for i in range(2, size):
            # 依次 进行判断，获取其最大的那个。
            first, second = second, max(first + nums[i], second)
        
        return second
```

***
## Review
> [msyql 使用B+树，为什么 MongoDB 使用 B树](https://medium.com/@mena.meseha/what-is-the-difference-between-mysql-innodb-b-tree-index-and-hash-index-ed8f2ce66d69)
### 概述
    文章主要对比了 B+树 B树 Hash索引 这几种优劣
    * B+ 索引，数据存储在 叶子结点，非叶子 节点仅仅是作为一个 索引，同时 查询的效率固定为 login(N)。
      同时使用了块读取，提高了数据索引范围。
    * B树 是 每一个 节点 都会有 file data 这样就能够快速的找到数据，同时他的 叶子节点是随机的，所以他的 索引时间可能会是不固定的。
      最低是 O(1)。这种也是为了 适应 快速索引，横向扩展提出的方法。
    * Hash 索引，这种更多的是等值索引，同时因为有Hash 算法，可以快速找到相关的数据，这种情况具有非常快的方式。但是在范围索引和key相同的情况下，
      这种索引的性能会进行全表扫描。
    
### 思考
    文中给出的解释是：因为 mongodb 的数据结构 存储的是 bson(like json)，需要 考虑 他的可扩展性 所以使用 B树，但是个人感觉还是太过于 肤浅。
    没有深入到核心，从数据结构/存储方式/应用场景等方式提出一些高效建议。后续还是需要查看一些更加多的数据同步尝试完成数据同步。
    
***
## Tip
> [Kafka send batch 提高吞吐量](https://kafka-python.readthedocs.io/en/master/apidoc/KafkaProducer.html)
###概述
     python pykafka 2.2 使用的是异步batch 推送到Kafka，以此来提高系统的吞吐量，首先会将这些数据同步到 buff 缓存中，
     只有在数据到达了临界值 or 时间达到了才会推送到Kafka 中，这样来提高系统的并发。
 
### 思考
    1. 这样是否会占用很大的内存？引起OOM？
        不会，应该这些默认是 1024K 大小，同时还有时间条件主动推送。
    2. 是否会有数据丢失的情况，比如出现僵尸进程，或者 主进程 被 kill ，导致的数据丢失？
        有可能存在，虽然是线程安全，但是也有可能因为CPU占满，调度的问题，这些都是会影响起推送的性能。在看了一部分源码后，
        发现最后的接收到 -9 信号后，就会将数据 flush to  kafka ，保证数据的一致性。
        
    3. 这种方式，是否可以推广到其他的应用上？
        类似于 python 迭代器 应用在，读取1Tfile ，但是内存不足的情况下，需要使用生成器的方式逐步将数据同步，保证数据的一致性。
    

***
## Share
> [python Flask 源码解析，提高系统认知](https://juejin.im/post/5a32513ff265da430f321f3d)

### 概述
    这里讲解了 python Flask 框架的一些工作原理，是基于werkzeug 实现的 WSGI 接口的封装，
    * 首先实现了 URL method 映射 ，实现了 url to method 实现 
    * 底层是使用 HTTPServer.TCPServer 进行 tcp 接口转发和兼容。
    * 这里使用的是 栈 的方式 提高了 并发量，
    * 请求收到后，就会将起数据 推送到  栈 中，保证数据的 有效性。

