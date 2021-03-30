```
# ARTS Week 35

> ![](https://github.com/Carmenliukang/ARTS/blob/master/image/35/1.jpg)
>> 凡是过往，皆为序章

***

## Algoithm

> [零钱兑换 II](https://leetcode-cn.com/problems/coin-change-2/)

### 概述

给定不同面额的硬币和一个总金额。写出函数来计算可以凑成总金额的硬币组合数。假设每一种面额的硬币有无限个。

示例 1:

输入: amount = 5, coins = [1, 2, 5]
输出: 4 解释: 有四种方式可以凑成总金额:
5=5 5=2+2+1 5=2+1+1+1 5=1+1+1+1+1 示例 2:

输入: amount = 3, coins = [2]
输出: 0 解释: 只用面额2的硬币不能凑成总金额3。 示例 3:

输入: amount = 10, coins = [10]
输出: 1

注意:

你可以假设：

0 <= amount (总金额) <= 5000 1 <= coin (硬币面额)<= 5000 硬币种类不超过 500 种 结果符合 32 位符号整数

### coding

````````

```python
class Solution:
    def change(self, amount: int, coins: list[int]) -> int:
        dp = [0] * (amount + 1)
        dp[0] = 1
        
        for coin in coins:
            for x in range(coin, amount + 1):
                dp[x] += dp[x - coin]
        return dp[amount]
```

***

## Review

[comment]: <> (> [todo]&#40;todo&#41;)

### 概述

todo

***

## Tip

> [Redis-缓冲区](https://time.geekbang.org/column/article/291277)

### 概述

S/C 结构中，缓冲区是为了能够暂存 输入/输出 的命令，这些不能过大。

### 客户端输入和输出缓冲区

1. 输入缓冲区:会先把客户端发送过来的命令暂存起来，Redis 主线程再从输入缓冲区中读取命令，进行处理。当 Redis 主线程处理完数据后，会把结果写入到
2. 输出缓冲区:当 Redis 主线程处理完数据后，会把结果写入到输出缓冲区，再通过输出缓冲区返回给客户端

![](https://github.com/Carmenliukang/ARTS/blob/master/image/35/2.jpg)

#### 输入缓冲区溢出

出现的原因：

1. 写入了 bigkey，比如一下子写入了多个百万级别的集合类型数据；
2. 服务器端处理请求的速度过慢，例如，Redis 主线程出现了间歇性阻塞，无法及时处理正常发送的请求，导致客户端发送的请求在缓冲区越积越多。

查看方式

```shell

CLIENT LIST
id=5 addr=127.0.0.1:50487 fd=9 name= age=4 idle=0 flags=N db=0 sub=0 psub=0 multi=-1 qbuf=26 qbuf-free=32742 obl=0 oll=0 omem=0 events=r cmd=client
```

另一类是与输入缓冲区相关的三个参数：

* cmd，表示客户端最新执行的命令。这个例子中执行的是 CLIENT 命令
* qbuf，表示输入缓冲区已经使用的大小。这个例子中的 CLIENT 命令已使用了 26 字节大小的缓冲区。
* qbuf-free，表示输入缓冲区尚未使用的大小。这个例子中的 CLIENT 命令还可以使用 32742 字节的缓冲区。
* qbuf 和 qbuf-free 的总和就是，Redis 服务器端当前为已连接的这个客户端分配的缓冲区总大小。

这个例子中总共分配了 26 + 32742 = 32768 字节，也就是 32KB 的缓冲区。

redis server 默认设置maxmemory 4。如果超过，那么就会出发内存淘汰机制。

##### 解决问题

1. 缓冲区调大
2. 从数据命令的发送和处理速度入手。

方法1，暂时不可取，因为redis未开放出相关的参数，默认设置未1G.

#### 输出缓冲区溢出

输出缓冲包含两种:

1. 16KB 固定缓冲大小,OK,err
2. 动态增加缓冲空间，暂存大小可变的响应结果

出现这种情况的原因：

1. bigkey
2. MONITOR
3. 缓冲区大小设置不合理

##### MONITOR

命令会持续监测redis命令：不推荐生产环境
![](https://github.com/Carmenliukang/ARTS/blob/master/image/35/3.png)

##### 缓存区大小设置

我们可以通过 client-output-buffer-limit 配置项，来设置缓冲区的大小

1. 设置缓冲区大小的上限阈值；
2. 设置输出缓冲区持续写入数据的数量上限阈值，和持续写入数据的时间的上限阈值。

```shell

client-output-buffer-limit normal 0 0 0
```

normal 表示当前设置的是普通客户端，

* 第 1 个 0 设置的是缓冲区大小限制
* 第 2 个 0 和第 3 个 0 分别表示缓冲区持续写入量限制和持续写入时间限制。

使用不同方式进行同步分析

* 普通客户端请求，可以设置成0，0，0
* 如果是其他的模式，可以通过手动设定相关的缓冲区大小

***

## Share

> [链表题目总结](https://github.com/Carmenliukang/ARTS/blob/master/week35.md#share)

### 概述

链表题目整体框架：

```

# 将生成的新的 head 指针引用
# python 中的一切都是引用，所以这里会引用起固定的内存地址
result = head

while head:
	# 相关的函数处理，比如中间某一个节点的删除/多个树枝删除
	# 可以是多个指针，同时中间增加参数
	
	if head.val==num:
	    head.val = head.next.val
	    head.next = head.next.next
	    
  head = head.next

```

#### 快慢指针

```
slow = head
fast = head.next

while fast:
    fast = fast.next
    slow = slow.next

```