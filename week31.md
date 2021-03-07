# ARTS Week 31

> ![](https://github.com/Carmenliukang/ARTS/blob/master/image/31/1.jpg)
>> 世界上任何书籍都不能带给你好运，但是它们能让你悄悄成为你自己 --赫尔曼黑塞

***

## Algoithm

> [解码方法](https://leetcode-cn.com/problems/decode-ways/)

### 概述

一条包含字母 A-Z 的消息通过以下映射进行了 编码 ：

    'A' -> 1
    'B' -> 2 ...
    'Z' -> 26 要 解码 已编码的消息，所有数字必须基于上述映射的方法，反向映射回字母（可能有多种方法）。例如，"111" 可以将 "1" 中的每个 "1" 映射为 "A" ，从而得到 "AAA" ，或者可以将 "11" 和 "1"
    （分别为 "K" 和 "A" ）映射为 "KA" 。注意，"06" 不能映射为 "F" ，因为 "6" 和 "06" 不同。

给你一个只含数字的 非空 字符串 num ，请计算并返回 解码 方法的 总数 。

题目数据保证答案肯定是一个 32 位 的整数。

示例 1：

    输入：s = "12"
    输出：2
    解释：它可以解码为 "AB"（1 2）或者 "L"（12）。

示例 2：

    输入：s = "226"
    输出：3
    解释：它可以解码为 "BZ" (2 26), "VF" (22 6), 或者 "BBF" (2 2 6) 。

示例 3：

    输入：s = "0"
    输出：0
    解释：没有字符映射到以 0 开头的数字。含有 0 的有效映射是 'J' -> "10" 和 'T'-> "20" 。由于没有字符，因此没有有效的方法对此进行解码，因为所有数字都需要映射。

示例 4：

    输入：s = "06"
    输出：0
    解释："06" 不能映射到 "F" ，因为字符串开头的 0 无法指向一个有效的字符。

提示：

1 <= s.length <= 100 s 只包含数字，并且可能包含前导零。

### 解析

!()[https://github.com/Carmenliukang/ARTS/blob/master/image/31/2.png]

### coding

```python
class Solution:
    def numDecodings(self, s: str) -> int:
        if not s or s[0] == "0":
            return 0
        # 使用 dp 算法同步修改策略方式
        size = len(s)
        dp = [0] * (size + 1)
        dp[0] = dp[1] = 1

        for i in range(1, size):
            if s[i] == "0":
                if s[i - 1] == "1" or s[i - 1] == "2":
                    dp[i + 1] = dp[i - 1]
                else:
                    return 0

            else:
                if s[i - 1] == "1" or (s[i - 1] == "2" and s[i] <= "6"):
                    dp[i + 1] = dp[i] + dp[i - 1]
                else:
                    dp[i + 1] = dp[i]

        return dp[-1]

```

***

## Review

> [Redis-异步机制：如何避免单线程模型的阻塞？](https://time.geekbang.org/column/article/285000)

### 概述

Redis 性能：

* Redis 内部的阻塞式操作；
* CPU 核和 NUMA 架构的影响；
* Redis 关键系统配置；
* Redis 内存碎片；
* Redis 缓冲区。

* 客户端：网络 IO，键值对增删改查操作，
* 数据库操作；磁盘：生成 RDB 快照，记录 AOF 日志，AOF 日志重写；
* 主从节点：主库生成、传输 RDB 文件，从库接收 RDB 文件、清空数据库、加载 RDB 文件；
* 切片集群实例：向其他实例传输哈希槽信息，数据迁移。

![](https://github.com/Carmenliukang/ARTS/blob/master/image/31/3.jpg)


#### 客户端
1. 网络，但是有 io多路复用，这些都比较低
2. 复杂的操作：
    1. 第一个阻塞点：集合全量查询和聚合操作。
    2. 复杂删除操作
        其实，删除操作的本质是要释放键值对占用的内存空间。你可不要小瞧内存的释放过程。释放内存只是第一步，为了更加高效地管理内存空间，在应用程序释放内存时，操作系统需要把释放掉的内存块插入一个空闲内存块的链表，以便后续进行管理和再分配。这个过程本身需要一定时间，而且会阻塞当前释放内存的应用程序，所以，如果一下子释放了大量内存，空闲内存块链表操作时间就会增加，相应地就会造成 Redis 主线程的阻塞。
    3. bigkey 删除操作
    4. 清空数据库
![](https://github.com/Carmenliukang/ARTS/blob/master/image/31/4.jpg)

#### 和磁盘交互时的阻塞点
1. AOF 日志
2. RDB 日志，影响比较小

### 阻塞操作异步
1. 异步执行
![](https://github.com/Carmenliukang/ARTS/blob/master/image/31/5.jpg)
   

### 异步的子线程机制

Redis 主线程启动后，会使用操作系统提供的 pthread_create 函数创建 3 个子线程，分别由它们负责 AOF 日志写操作、键值对删除以及文件关闭的异步执行。
![](https://github.com/Carmenliukang/ARTS/blob/master/image/31/6.jpg)

***

## Tip

> [todo](todo)

### 概述

todo

***

## Share

> [动态规划相关算法题目解析](https://github.com/Carmenliukang/ARTS/blob/master/week31.md#share)

### 概述

todo