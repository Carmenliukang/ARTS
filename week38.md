# ARTS Week 38

> ![](https://github.com/Carmenliukang/ARTS/blob/master/image/38/1.jpeg)
>> 从零开始

***

## Algoithm

> [单词拆分](https://leetcode-cn.com/problems/word-break)

### 概述

给定一个非空字符串 s 和一个包含非空单词的列表 wordDict，判定s是否可以被空格拆分为一个或多个在字典中出现的单词。

说明：

拆分时可以重复使用字典中的单词。 你可以假设字典中没有重复的单词。

示例 1：

    输入: s = "leetcode", wordDict = ["leet", "code"]
    输出: true 
    解释: 返回 true 因为 "leetcode" 可以被拆分成 "leet code"。

示例 2：

    输入: s = "applepenapple", wordDict = ["apple", "pen"]
    输出: true 
    解释: 返回 true 因为 "applepenapple" 可以被拆分成 "apple pen apple"。 注意你可以重复使用字典中的单词。

示例 3：

    输入: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
    输出: false

### 分析

这种判断是否可以组合，都是一种极值判断，大概的方向是递归问题。

那么接下来还是老三步：

1. 状态
2. 选择
3. 函数

#### 状态

首先这里的状态可以从问题入手，其中需要到最后是否满足，那么其中每一都可以判断是否满足，这样状态就变成了，从0-i 中，wordDict 是否满足？

#### 选择

1. 特殊情况
    1. 如果均为空，那么为空的情况，结果肯定为真。
2. 多种选择
    1. 0-i 判断
    2. 中间增加一个 j，从0-j，j-i 均满足，得到转换 dp[i]=dp[j] && check(s[j..i−1])

#### 函数

1. dp 定义
2. 状态选择
3. 返回结果

### code

```python3

from typing import List


class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        # 状态 当前i,j 是否为0
        # 选择 0-i 中进行切割，0-j,j-i 中可以切割。
        size = len(s)
        dp = [False] * (size + 1)
        dp[0] = True
        # 使用哈希表快速同步
        word_map = {i: True for i in wordDict}
        # 使用dp算法，进行计算
        for i in range(1, size + 1):
            for j in range(i):
                if dp[j] and word_map.get(s[j:i]):
                    dp[i] = True
                    break
        return dp[-1]

```

### 总结

1. 关于状态
    1. 动态问题 可以从问题入手，问题的最终判断即为单个的状态
2. 关于选择
    1. 选择可以通过多层循环完成，从中的状态进行选择

***

## Review

> [FastAPI Documentation](https://fastapi.tiangolo.com/)

### 概述

FastAPI is a modern, fast (high-performance), web framework for building APIs with Python 3.6+ based on standard Python
type hints.

The key features are:

* **Fast**: Very high performance, on par with NodeJS and Go (thanks to Starlette and Pydantic). One of the fastest
  Python frameworks available.

* **Fast to code**: Increase the speed to develop features by about 200% to 300%. *

* **Fewer bugs**: Reduce about 40% of human (developer) induced errors. *
* **Intuitive**: Great editor support. Completion everywhere. Less time debugging.
* **Easy**: Designed to be easy to use and learn. Less time reading docs.
* **Short**: Minimize code duplication. Multiple features from each parameter declaration. Fewer bugs.
* **Robust**: Get production-ready code. With automatic interactive documentation. Standards-based: Based on (and fully
  compatible with) the open standards for APIs: OpenAPI (previously known as Swagger) and JSON Schema.

***

## Tip

> [我查这么多数据，会不会把数据库内存打爆？](https://time.geekbang.org/column/article/79407)

### 概述

问题：

我的主机内存只有 100G，现在要对一个 200G 的大表做全表扫描，会不会把数据库主机的内存用光了？

分成两个维度：

1. 全表扫描对于 Server 层的影响
2. 全表扫描对 InnoDB 的影响

### Server 层

示例：

```shell
mysql -h$host -P$port -u$user -p$pwd -e "select * from db1.t" > $target_file
```

#### 流程

实际上，服务端并不需要保存一个完整的结果集。取数据和发数据的流程是这样的：

1. 获取一行，写到 net_buffer 中。这块内存的大小是由参数 net_buffer_length 定义的，默认是 16k。
2. 重复获取行，直到 net_buffer 写满，调用网络接口发出去。
3. 如果发送成功，就清空 net_buffer，然后继续取下一行，并写入 net_buffer。
4. 如果发送函数返回 EAGAIN 或 WSAEWOULDBLOCK，就表示本地网络栈（socket send buffer）写满了，进入等待。直到网络栈重新可写，再继续发送。

![](https://github.com/Carmenliukang/ARTS/blob/master/image/38/2.webp)

**Mysql是边读边发**，但是如果客户端接收比较慢，就会导致事务执行时间变长。状态一直显示 **“Sending to client”**

![](https://github.com/Carmenliukang/ARTS/blob/master/image/38/3.webp)

#### 建议

对于正常的线上业务来说，如果一个查询的返回结果不会很多的话，我都建议你使用 mysql_store_result 这个接口，直接把查询结果保存到本地内存。

#### **“Sending data”**

这一状态显示数据可能会在链接中等各种状态

![](https://github.com/Carmenliukang/ARTS/blob/master/image/38/4.webp)

![](https://github.com/Carmenliukang/ARTS/blob/master/image/38/5.webp)

### InnoDB

1. WAL机制
2. BP管理
3. LRU 算法以及优化

#### WAL机制

Write-Ahead Logging，先写日志，再写磁盘。

具体来说，当有一条记录需要更新的时候，InnoDB 引擎就会先把记录写到 redo log（粉板）里面，并更新内存，这个时候更新就算完成了。同时，InnoDB
引擎会在适当的时候，将这个操作记录更新到磁盘里面，而这个更新往往是在系统比较空闲的时候做，这就像打烊以后掌柜做的事。

#### BP管理

核心因素是命中率

show engine innodb status

![](https://github.com/Carmenliukang/ARTS/blob/master/image/38/6.webp)

#### LRU算法

最近最久未使用

基本的LRU算法

1. 在图 6 的状态 1 里，链表头部是 P1，表示 P1 是最近刚刚被访问过的数据页；假设内存里只能放下这么多数据页；
2. 这时候有一个读请求访问 P3，因此变成状态 2，P3 被移到最前面；
3. 状态 3 表示，这次访问的数据页是不存在于链表中的，所以需要在 Buffer Pool 中新申请一个数据页 Px，加到链表头部。但是由于内存已经满了，不能申请新的内存。于是，会清空链表末尾 Pm 这个数据页的内存，存入 Px
   的内容，然后放到链表头部。
4. 从效果上看，就是最久没有被访问的数据页 Pm，被淘汰了。

![](https://github.com/Carmenliukang/ARTS/blob/master/image/38/7.webp)

那么，按照这个算法扫描的话，就会把当前的 Buffer Pool 里的数据全部淘汰掉，存入扫描过程中访问到的数据页的内容。也就是说 Buffer Pool 里面主要放的是这个历史数据表的数据。

对于一个正在做业务服务的库，这可不妙。你会看到，Buffer Pool 的内存命中率急剧下降，磁盘压力增加，SQL 语句响应变慢。

改进后的一些修改

![](https://github.com/Carmenliukang/ARTS/blob/master/image/38/8.webp)

在 InnoDB 实现上，按照 5:3 的比例把整个 LRU 链表分成了 young 区域和 old 区域。图中 LRU_old 指向的就是 old 区域的第一个位置，是整个链表的 5/8 处。也就是说，靠近链表头部的 5/8 是
young 区域，靠近链表尾部的 3/8 是 old 区域。改进后的 LRU 算法执行流程变成了下面这样。

1. 图 7 中状态 1，要访问数据页 P3，由于 P3 在 young 区域，因此和优化前的 LRU 算法一样，将其移到链表头部，变成状态 2。
2. 之后要访问一个新的不存在于当前链表的数据页，这时候依然是淘汰掉数据页 Pm，但是新插入的数据页 Px，是放在 LRU_old 处。
3. 处于 old 区域的数据页，每次被访问的时候都要做下面这个判断：
    1. 若这个数据页在 LRU 链表中存在的时间超过了 1 秒，就把它移动到链表头部；
    2. 如果这个数据页在 LRU 链表中存在的时间短于 1 秒，位置保持不变。1 秒这个时间，是由参数 innodb_old_blocks_time 控制的。其默认值是 1000，单位毫秒。

***

## Share

> [关于业务逻辑抽象](https://github.com/Carmenliukang/ARTS/blob/master/week38.md#share)

### 概述

**业务抽象** : 整体可以设计成 底层无状态抽象，中层业务组建抽象，业务线开发

首先从公司角度出发，对于创业公司/蓝海时代，需要快速切入不同场景，能够实现快速接入。这种模式下需要走广度优先(DFS)策略。所以需要对自身的产品进行较为抽象的逻辑思考。

以机器人为例：

1. 位置
2. 信息
3. 动作

这种抽象能够快速的搭建，底层的逻辑抽象。实现一种最基础的逻辑抽象，无状态、无业务的底层抽象。

中层是业务组建抽象：将中层不同业务需要的公共组件，实现可插播方式快速支撑。例如：支付模块/鉴权模块/基础位置模块/N叉树模块 等等。

业务线开发：这里会有很多定制化开发项目，例如：第三方对接/特殊推送/定制化回调等。
