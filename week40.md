# ARTS Week 40

> ![](https://github.com/Carmenliukang/ARTS/blob/master/image/40/douglas-rivera-_oQH-2lv6Cw-unsplash.jpg)
>> 从零开始

***

## Algoithm

> [机器人的运动范围](https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof)

### 概述

地上有一个m行n列的方格，从坐标 [0,0] 到坐标 [m-1,n-1] 。一个机器人从坐标 [0, 0]
的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]
，因为3+5+3+8=19。请问该机器人能够到达多少个格子？

示例 1：

    输入：m = 2, n = 3, k = 1
    输出：3

示例 2：

    输入：m = 3, n = 1, k = 0
    输出：1

提示：

    1 <= n,m <= 100
    0 <= k<= 20

***

### 分析

这里可以使用 DP方法，确定状态，分析转换，函数表达

#### 状态

每一个节点是否符合要求，符合为1，不符合为0

#### 转移方程式

(i-1,j || i,j-1 满足条件) && 同时当前满足条件

### code

```python

class Solution:
    def movingCount(self, m: int, n: int, k: int) -> int:
        # 这里使用的是最基础的逻辑修改
        vis = set()
        vis.add((0, 0))
        for i in range(m):
            for j in range(n):
                if ((i - 1, j) in vis or (i, j - 1) in vis) and self.digitsum(i) + self.digitsum(j) <= k:
                    vis.add((i, j))
        return len(vis)

    def digitsum(self, n):
        # 用于计算每一个位置数字之和
        ans = 0
        while n:
            ans += n % 10
            n //= 10
        return ans
```

### 总结

这个题目初期分析较为复杂，其实需要这种，M*N 的方式，其实都可以从最初的点的状态进行判断。

## Review

> [推荐阅读：分布式数据调度相关论文](https://time.geekbang.org/column/article/2421)

### 概述

主要介绍了分布式 数据调度 相关论文的链接

[Bigtable: A Distributed Storage System for Structured Data](https://static.googleusercontent.com/media/research.google.com/en//archive/bigtable-osdi06.pdf)

[The Chubby lock service for loosely-coupled distributed systems](https://static.googleusercontent.com/media/research.google.com/en//archive/chubby-osdi06.pdf)

[The Google File System](https://static.googleusercontent.com/media/research.google.com/en//archive/gfs-sosp2003.pdf)

[MapReduce: Simplifed Data Processing on Large Clusters](https://static.googleusercontent.com/media/research.google.com/en//archive/mapreduce-osdi04.pdf)

### 逻辑钟和向量钟

后面，业内又搞出来一些工程上的东西，比如 Amazon 的 DynamoDB，其论文Dynamo: Amazon’s Highly Available Key Value Store 的影响力也很大。这篇论文中讲述了 Amazon 的
DynamoDB 是如何满足系统的高可用、高扩展和高可靠要求的，其中还展示了系统架构是如何做到数据分布以及数据一致性的。GFS 采用的是查表式的数据分布，而 DynamoDB
采用的是计算式的，也是一个改进版的通过虚拟结点减少增加结点带来数据迁移的一致性哈希。另外，这篇论文中还讲述了一个 NRW 模式用于让用户可以灵活地在 CAP 系统中选取其中两项，这使用到了 Vector
Clock——向量时钟来检测相应的数据冲突。最后还介绍了使用 Handoff 的机制对可用性的提升。这篇文章中有几个关键的概念，一个是 Vector Clock，另一个是 Gossip
协议。提到向量时钟就需要提一下逻辑时钟。所谓逻辑时间，也就是在分布系统中为了解决消息有序的问题，由于在不同的机器上有不同的本地时间，这些本地时间的同步很难搞，会导致消息乱序。于是 Paxos
算法的发明人兰伯特（Lamport）搞了个向量时钟，每个系统维护一个本地的计数器，这就是所谓的逻辑时钟。每执行一个事件（例如向网络发送消息，或是交付到应用层）都对这个计数器做加 1
操作。当跨系统的时候，在消息体上附着本地计算器，当接收端收到消息时，更新自己的计数器（取对端传来的计数器和自己当成计数器的最大值），也就是调整自己的时钟。逻辑时钟可以保证，如果事件 A 先于事件 B，那么事件 A 的时钟一定小于事件 B
的时钟，但是返过来则无法保证，因为返过来没有因果关系。所以，向量时钟解释了因果关系。向量时钟维护了数据更新的一组版本号（版本号其实就是使用逻辑时钟）。假如一个数据需要存在三个结点上 A、B、C。那么向量维度就是
3，在初始化的时候，所有结点对于这个数据的向量版本是[A:0, B:0, C:0]。当有数据更新时，比如从 A 结点更新，那么，数据的向量版本变成[A:1, B:0, C:0]，然后向其他结点复制这个版本，其在语义上表示为我当前的数据是由
A 结果更新的，而在逻辑上则可以让分布式系统中的数据更新的顺序找到相关的因果关系。这其中的逻辑关系，你可以看一下 马萨诸塞大学课程 D

***

## Tip

> [30 | 编程范式游记（1）- 起源](https://time.geekbang.org/column/article/301)

### 概述

起源说明

### C开始

语言特性

1. C 语言是一个静态弱类型语言，在使用变量时需要声明变量类型，但是类型间可以有隐式转换；
2. 不同的变量类型可以用结构体（struct）组合在一起，以此来声明新的数据类型；
3. C 语言可以用 typedef 关键字来定义类型的别名，以此来达到变量类型的抽象；
4. C 语言是一个有结构化程序设计、具有变量作用域以及递归功能的过程式语言；
5. C 语言传递参数一般是以值传递，也可以传递指针；
6. 通过指针，C 语言可以容易地对内存进行低级控制，然而这加大了编程复杂度；
7. 编译预处理让 C语言的编译更具有弹性，比如跨平台。

#### 数据类型与现实世界的类比

与现实世界类比一下，数据类型就好像螺帽一样，有多种接口方式：平口的、十字的、六角的等，而螺丝刀就像是函数，或是用来操作这些螺丝的算法或代码。我们发现，这些不同类型的螺帽（数据类型），需要我们为之适配一堆不同的螺丝刀。

而且它们还有不同的尺寸（尺寸就代表它是单字节的，还是多字节的，比如整型的 int、long，浮点数的 float 和 double），这样复杂度一下就提高了，最终导致电工（程序员）工作的时候需要带下图这样的一堆工具。

这就是类型为编程带来的问题。要解决这个问题，我们还是来看一下现实世界。

你应该见过下面图片中的这种经过优化的螺丝刀，上面手柄是一样的，拧螺丝的动作也是一样的，只是接口不一样。每次我看到这张图片的时候就在想，这密密麻麻的看着有 40 多种接口，不知道为什么人类世界要干出这么多的花样，你们这群人类究竟是要干什么啊。

我们可以看到，无论是传统世界，还是编程世界，我们都在干一件事情，什么事呢？**那就是通过使用一种更为通用的方式，用另外的话说就是抽象和隔离，让复杂的“世界”变得简单一些。**

然而，要做到抽象，对于 C 语言这样的类型语言来说，首先要拿出来讲的就是抽象类型，这就是所谓的泛型编程。

另外，我们还要注意到，在编程世界里，对于 C 语言来说，类型还可以转换。编译器会使用一切方式来做类型转换，因为类型转换有时候可以让我们编程更方便一些，也让相近的类型可以做到一点点的泛型。

然而，对于 C 语言的类型转换，是会出很多问题的。比如说，传给我一个数组，这个数组本来是 double 型的，或者是 long 型 64 位的，但是如果把数组类型强转成 int，那么就会出现很多问题，因为这会导致程序遍历数组的步长不一样了。

比如：一个 double a[10] 的数组，a[2] 意味着 a + sizeof(double) * 2。如果你把 a 强转成 int，那么 a[2] 就意味着 a + sizeof(int) * 2。我们知道 sizeof(double) 是 8，而 sizeof(int) 是 4。于是访问到了不同的地址和内存空间，这就导致程序出现严重的问题。


### C 语言的泛型

#### 一个泛型的示例 - swap 函数


```c

void swap(void* x, void* y, size_t size)
{
     char tmp[size];
     memcpy(tmp, y, size);
     memcpy(y, x, size);
     memcpy(x, tmp, size);
}



#define swap(x, y, size) {\
  char temp[size]; \
  memcpy(temp, &y, size); \
  memcpy(&y,   &x, size); \
  memcpy(&x, temp, size); \
}
```

### 总结

C 语言设计目标是提供一种能以简易的方式编译、处理低层内存、产生少量的机器码以及不需要任何运行环境支持便能运行的编程语言。C 语言也很适合搭配汇编语言来使用。C 语言把非常底层的控制权交给了程序员，它设计的理念是：

* 相信程序员；
* 不会阻止程序员做任何底层的事；
* 保持语言的最小和最简的特性；
* 保证 C 语言的最快的运行速度，那怕牺牲移值性。




***

## Share

> [sqlalchemy async clint 过多的问题](https://github.com/Carmenliukang/ARTS/blob/master/week40.md#share)

### 概述

遇到一个问题 使用 async sqlalchemy 。最后导致数据库连接 上万，直接无法新建链接。

[Slow introspection when using multiple custom types in a query #530](https://github.com/MagicStack/asyncpg/issues/530)

因为原生async sqlalchemy 是没有 自省的，所以每次都是一个新的 client。解决办法就是 增加一个参数：

这种方式可以解决该问题：

await asyncpg.create_pool(..., server_settings={'jit': 'off'})
