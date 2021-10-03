# ARTS Week 41

> ![](https://github.com/Carmenliukang/ARTS/blob/master/image/41/jed-villejo-DOM6hsFxIX4-unsplash.jpg)
>> 总是有人要赢的，那为什么不能是我呢？ ——科比·布莱恩特

***

## Algoithm

> [树的直径](https://leetcode-cn.com/problems/tree-diameter/)

### 概述

给你这棵「无向树」，请你测算并返回它的「直径」：这棵树上最长简单路径的 边数。

我们用一个由所有「边」组成的数组 edges 来表示一棵无向树，其中 edges[i] = [u, v] 表示节点 u 和 v 之间的双向边。

树上的节点都已经用 {0, 1, ..., edges.length} 中的数做了标记，每个节点上的标记都是独一无二的。

示例 1：

![](https://github.com/Carmenliukang/ARTS/blob/master/image/41/4.png)

输入：edges = [[0,1],[0,2]]

输出：2 

解释： 这棵树上最长的路径是 1 - 0 - 2，边数为 2。

示例 2：

![](https://github.com/Carmenliukang/ARTS/blob/master/image/41/5.png)
   
输入：edges = [[0,1],[1,2],[2,3],[1,4],[4,5]]

输出：4 

解释： 这棵树上最长的路径是 3 - 2 - 1 - 4 - 5，边数为 4。`

提示：

* 0 <= edges.length < 10^4
* edges[i][0] != edges[i][1]
* 0 <= edges[i][j] <= edges.length
* edges 会形成一棵无向树

来源：力扣（LeetCode） 链接：https://leetcode-cn.com/problems/tree-diameter
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

### code

```python


from typing import List
from collections import defaultdict


class Solution:
    # 使用两次 BFS 方式计算
    def treeDiameter(self, edges: List[List[int]]) -> int:
        n = len(edges)
        adjVex = defaultdict(list)  # 邻接表
        for x, y in edges:  # 初始化邻接表，建图
            adjVex[x].append(y)
            adjVex[y].append(x)

        que = [0]
        visited = [False for _ in range(n + 1)]
        visited[0] = True
        cur = 0  # 全局变量，好记录第一次BFS最后一个点的ID
        while que:
            cur_len = len(que)
            for _ in range(cur_len):
                cur = que.pop(0)
                for nxt in adjVex[cur]:
                    if visited[nxt] == False:
                        visited[nxt] = True  # 进队时visit和出队时visit都可以
                        que.append(nxt)
        visited = [False for _ in range(n + 1)]
        que = [cur]  # 第一次最后一个点，作为第二次BFS的起点
        visited[cur] = True
        level = -1  # 记好距离
        while que:
            cur_len = len(que)
            level += 1
            for _ in range(cur_len):
                cur = que.pop(0)
                for nxt in adjVex[cur]:
                    if visited[nxt] == False:
                        visited[nxt] = True
                        que.append(nxt)
        return level
```

***

## Review

> [编程范式游记（2）- 泛型编程](https://time.geekbang.org/column/article/303)

### C++ 泛型编程

理想情况下，算法应是和数据结构以及类型无关的，各种特殊的数据类型理应做好自己分内的工作，算法只关心一个标准的实现。
**而对于泛型的抽象，我们需要回答的问题是，如果我们的数据类型符合通用算法，那么对数据类型的最小需求又是什么呢？**

C++如何回答这个问题:

1. 它通过类的方式来解决。
    * 类里面会有构造函数、析构函数表示这个类的分配和释放。
    * 还有它的拷贝构造函数，表示了对内存的复制
    * 还有重载操作符，像我们要去比较大于、等于、不等于。

2. 通过模板达到类型和算法的妥协。
    * 模板有点像 DSL，模板的特化会根据使用者的类型在编译时期生成那个模板的代码。
    * 模板可以通过一个虚拟类型来做类型绑定，这样不会导致类型转换时的问题。

3. 通过虚函数和运行时类型识别。
    * 虚函数带来的多态在语义上可以支持“同一类”的类型泛型。
    * 运行时类型识别技术可以做到在泛型时对具体类型的特殊处理。

#### C++ 泛型编程的示例 - Search 函数

顺序性结构查询：array、set、queue、list 和 link等

非顺序性查询：如哈希表 hash table，二叉树 binary tree、图 graph 等

C语言版本：

```
int search(void* a, size_t size, void* target, 
  size_t elem_size, int(*cmpFn)(void*, void*) )
{
  for(int i=0; i<size; i++) {
    if ( cmpFn (a + elem_size * i, target) == 0 ) {
      return i;
    }
  }
  return -1;
}

```

C++版本

```

template<typename T, typename Iter>
Iter search(Iter pStart, Iter pEnd, T target) 
{
  for(Iter p = pStart; p != pEnd; p++) {
    if ( *p == target ) 
      return p;
  }
  return NULL;
}
```

* 使用typename T抽象了数据结构中存储数据的类型。
* 使用typename Iter，这是不同的数据结构需要自己实现的“迭代器”，这样也就抽象掉了不同类型的数据结构。
* 然后，我们对数据容器的遍历使用了Iter中的++方法，这是数据容器需要重载的操作符，这样通过操作符重载也就泛型掉了遍历。
* 在函数的入参上使用了pStart和pEnd来表示遍历的起止。
* 使用*Iter来取得这个“指针”的内容。这也是通过重载 * 取值操作符来达到的泛型。

#### C++ 泛型编程示例 - Sum 函数

C语言版本：

```shell

long sum(int *a, size_t size) {
  long result = 0;
  for(int i=0; i<size; i++) {
    result += a[i];
  }
  return result;
}
```

C++版本

```shell

template<typename T, typename Iter>
T sum(Iter pStart, Iter pEnd) {
  T result = 0;
  for(Iter p=pStart; p!=pEnd; p++) {
    result += *p;
  }
  return result;  
}

```

关键问题在于 result = 0 ，已经设置其类型为 Int。如果类型不一样，那么会有异常转换

#### C++ 泛型编程的重要技术 - 迭代器

精简版本

```shell


template <class T>
class container {
public:
  class iterator {
  public:
    typedef iterator self_type;
    typedef T   value_type;
    typedef T*  pointer;
    typedef T&   reference;

    reference operator*();
    pointer operator->();
    bool operator==(const self_type& rhs)；
    bool operator!=(const self_type& rhs)；
    self_type operator++() { self_type i = *this; ptr_++; return i; }
    self_type operator++(int junk) { ptr_++; return *this; }
    ...
    ...
  private:
    pointer _ptr;
  };

  iterator begin();
  iterator end();
  ...
  ...
};
```

* 首先，一个迭代器需要和一个容器在一起，因为里面是对这个容器的具体的代码实现。
* 它需要重载一些操作符，比如：取值操作*、成员操作->、比较操作==和!=，还有遍历操作++，等等。
* 然后，还要typedef一些类型，比如value_type，告诉我们容器内的数据的实际类型是什么样子。
* 还有一些，如begin()和end()的基本操作。
* 我们还可以看到其中有一个pointer _ptr的内部指针来指向当前的数据（注意，pointer就是 T*）。

优化版本 Sum

```shell


template <class Iter>
typename Iter::value_type
sum(Iter start, Iter end, T init) {
  typename Iter::value_type result = init;
  while (start != end) {
    result = result + *start;
    start++;
  }
  return result;
}
```

***

## Tip

> [Dapr](https://docs.dapr.io/)

### 概述

**Dapr** 是一个可移植的、事件驱动的运行时，它使任何开发人员能够轻松构建出弹性的、无状态和有状态的应用程序，并可运行在云平台或边缘计算中，它同时也支持多种编程语言和开发框架。

![](https://github.com/Carmenliukang/ARTS/blob/master/image/41/1.png)

如今，我们正经历着上云浪潮。 开发人员习惯于 Web + 数据库应用架构(例如经典 3 层设计)，但对天然支持分布式的微服务应用架构却感觉陌生。 成为分布式系统专家很难，并且你也不需要这么做。
开发人员希望专注于业务逻辑，同时希望平台为其提供可伸缩的、弹性的、可维护的和云原生架构的其他功能。

这就是Dapr所要解决的。 Dapr 将构建微服务应用的 最佳实践 设计成开放、独立和模块化的方式，让你能够使用任意的开发语言和框架构建可移植的应用程序。 每个构建块都是完全独立的，您可以采用其中一个、多个或全部来构建你的应用。

此外，Dapr 是和平台无关的，这意味着您可以在本地、Kubernetes 集群或者其它集成 Dapr 的托管环境中运行应用程序。 这使得您能够在云平台和边缘计算中运行微服务应用。
使用 Dapr，您可以使用任何语言、框架轻松构建微服务应用，运行在任何地方。

### 云平台和边缘计算的微服务构建块

![](https://github.com/Carmenliukang/ARTS/blob/master/image/41/2.png)

在设计微服务应用时，需要考虑很多因素。 Dapr提供了一些常用功能的最佳实践，开发人员可以使用标准模式进行微服务应用的构建，并部署到任意环境中。 Dapr 通过提供分布式构建块来实现此目的。

每个构建块都是独立的，这意味着您可以采用其中一个、多个或全部来构建应用。 目前，可用的构建块如下：

1. 服务调用: 跨服务调用允许进行远程方法调用(包括重试)，不管处于任何位置，只需该服务托管于受支持的环境即可。
2. 状态管理: 独立的状态管理，使用键/值对作为存储机制，可以轻松的使长时运行、高可用的有状态服务和无状态服务共同运行在您的应用程序中。 状态存储是可插拔的，目前支持使用Azure CosmosDB、 Azure SQL Server、
   PostgreSQL,、AWS DynamoDB、Redis 作为状态存储介质。
3. 发布订阅: 发布事件和订阅主题。
4. 资源绑定: Dapr的Bindings是建立在事件驱动架构的基础之上的。通过建立触发器与资源的绑定，可以从任何外部源（例如数据库，队列，文件系统等）接收和发送事件，而无需借助消息队列，即可实现灵活的业务场景。
5. Actors: Actor模型 = 状态 + 行为 +
   消息。一个应用/服务由多个Actor组成，每个Actor都是一个独立的运行单元，拥有隔离的运行空间，在隔离的空间内，其有独立的状态和行为，不被外界干预，Actor之间通过消息进行交互，而同一时刻，每个Actor只能被单个线程执行，这样既有效避免了数据共享和并发问题，又确保了应用的伸缩性。
   Dapr 在Actor模式中提供了很多功能，包括并发，状态管理，用于 actor 激活/停用的生命周期管理，以及唤醒 actor 的计时器和提醒器。
6. 可观测性: Dapr记录指标，日志，链路以调试和监视Dapr和用户应用的运行状况。 Dapr支持分布式跟踪，其使用W3C跟踪上下文标准和开放式遥测技术，可以轻松地诊断在生产环境中服务间的网络调用，并发送到不同的监视工具。
7. 安全：Dapr 提供了密钥管理，支持与公有云和本地的Secret存储集成，以供应用检索使用。

#### Sidecar 架构

Dapr以 sidecar 架构的方式公开其API，可以是容器，也可以是进程，不需要应用代码包含任何 Dapr 运行时代码。 这使得 Dapr 与其他运行时的集成变得容易，在应用逻辑层面做了隔离处理，提高了可扩展性。

![](https://github.com/Carmenliukang/ARTS/blob/master/image/41/3.png)

***

## Share

> [Dapr FastAPI 快速开发](https://github.com/Carmenliukang/ARTS/blob/master/week41.md#share)

### 概述

因为项目较为紧急，同时需要切换dapr架构方式，所以打算使用FastAPI+Dapr 扩展插件快速开发

```python

# !/usr/bin/env python
# -*- coding: utf-8 -*-


import orjson
from fastapi import Body, FastAPI
from logzero import logger
from pydantic import Json
from dapr.actor import ActorId
from dapr.ext.fastapi import DaprActor

from .registry import get_actor, register_user_actors
from ..settings import DAPR__PUBSUB

app = FastAPI(title=f"App.UserActorService")
actor = DaprActor(app)


@app.on_event("startup")
async def startup_event():
    await register_user_actors(actor)
    logger.info("[ActorService] User Actor registered...")


@app.get("/dapr/subscribe")
async def subscribe():
    subscriptions = [
        {
            "pubsubname": DAPR__PUBSUB,
            "topic": "AppRiderEvent",
            "route": "sub_rider_event",
        },
        {
            "pubsubname": DAPR__PUBSUB,
            "topic": "AppCustomerEvent",
            "route": "sub_customer_event",
        },
    ]
    return subscriptions


@app.post("/sub_rider_event")
async def rider_event_subscriber(
        rider_event: Json[dict] = Body(..., alias="data", embed=True)
):
    logger.info(f"{rider_event=}")
    try:
        proxy1 = get_actor(
            "RiderActor",
            ActorId(f"RiderActor@r{rider_event['event_info']['rider_id']}"),
        )
        await proxy1.refresh_rider_event(orjson.dumps(rider_event))

    except Exception as e:
        logger.exception(f"[A.User] process #AppRiderEvent failed => {e}")

    return {"success": True}


@app.post("/sub_customer_event")
async def customer_event_subscriber(
        customer_event: Json[dict] = Body(..., alias="data", embed=True)
):
    try:
        proxy1 = get_actor(
            "CustomerActor",
            ActorId(f"CustomerActor@c{customer_event['event_info']['user_id']}"),
        )
        await proxy1.refresh_customer_event(orjson.dumps(customer_event))

    except Exception as e:
        logger.exception(f"[A.User] process #AppCustomerEvent failed => {e}")

    return {"success": True}


```
