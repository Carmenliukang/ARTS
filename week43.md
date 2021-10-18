# ARTS Week 43

> ![](https://github.com/Carmenliukang/ARTS/blob/master/image/43/peter-thomas-j2bxM61qXQ4-unsplash.jpg)
>> 猛兽总是独行，牛羊才成群结队。 ---鲁迅

***

## Algoithm

> [二叉树的最近公共祖先 IV](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree-iv)

### 概述

给定一棵二叉树的根节点 root 和 TreeNode 类对象的数组（列表） nodes，返回 nodes 中所有节点的最近公共祖先（LCA）。数组（列表）中所有节点都存在于该二叉树中，且二叉树中所有节点的值都是互不相同的。

我们扩展二叉树的最近公共祖先节点在维基百科上的定义：“对于任意合理的 i 值， n 个节点 p1 、 p2、...、 pn 在二叉树 T 中的最近公共祖先节点是后代中包含所有节点 pi
的最深节点（我们允许一个节点是其自身的后代）”。一个节点 x 的后代节点是节点 x 到某一叶节点间的路径中的节点 y。

示例 1:

![](https://github.com/Carmenliukang/ARTS/blob/master/image/43/1.png)

    输入: root = [3,5,1,6,2,0,8,null,null,7,4], nodes = [4,7]
    输出: 2
    解释: 节点 4 和 7 的最近公共祖先是 2。

示例 2:

![](https://github.com/Carmenliukang/ARTS/blob/master/image/43/2.png)

    输入: root = [3,5,1,6,2,0,8,null,null,7,4], nodes = [1]
    输出: 1
    解释: 单个节点的最近公共祖先是该节点本身。

示例 3:

![](https://github.com/Carmenliukang/ARTS/blob/master/image/43/3.png)

    输入: root = [3,5,1,6,2,0,8,null,null,7,4], nodes = [7,6,2,4]
    输出: 5
    解释: 节点 7、6、2 和 4 的最近公共祖先节点是 5。

示例 4:

![](https://github.com/Carmenliukang/ARTS/blob/master/image/43/4.png)

    输入: root = [3,5,1,6,2,0,8,null,null,7,4], nodes = [0,1,2,3,4,5,6,7,8]
    输出: 3
    解释: 树中所有节点的最近公共祖先是根节点。

提示:

* 树中节点个数的范围是 [1, 104] 。
* -109 <= Node.val <= 109
* 所有的 Node.val 都是互不相同的。
* 所有的 nodes[i] 都存在于该树中。
* 所有的 nodes[i] 都是互不相同的。

### 分析

类似父节点的查询题目，使用的是后序遍历

### code

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None


class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', nodes: 'List[TreeNode]') -> 'TreeNode':
        if root is None or root in nodes:
            return root

        left = self.lowestCommonAncestor(root.left, nodes)
        right = self.lowestCommonAncestor(root.right, nodes)

        if left and right:
            return root
        return left or right

```

***

## Review

> [You don’t need JWT anymore](https://medium.com/@bytesbay/you-dont-need-jwt-anymore-974aa6196976)

### 概述

A simpler way to authenticate users with web3 using signed messages

![](https://github.com/Carmenliukang/ARTS/blob/master/image/43/10.png)

It’s no secret that the Ethereum login will soon become a user standard and passwords will no longer be needed.
Nevertheless, dApp development is still a fairly young direction and many standards for their development are still set.

I myself started developing dApps using JWT. From the first project, I felt that authentication always becomes tricky
and that there must be something redundant in the process. After a couple of projects, I realized that the JWT itself is
redundant. Let me explain why.

Why do we need to get JWT? After all, even without it, you can reliably identify the user by taking the address from his
signature. Here’s how to simplify it:

![](https://github.com/Carmenliukang/ARTS/blob/master/image/43/11.png)

To simplify this flow even more, I made the web3-token module. To install it, use the command:

```shell
$ npm i web3-token

```

Let’s look at an example, starting with the client-side.

```shell

import Web3Token from 'web3-token';

// Connection to MetaMask wallet (you can actually use any wallet)
// you can even use ethersjs instead of web3
const web3 = new Web3(ethereum);
await ethereum.enable();

// getting address from which we will sign message
const address = (await web3.eth.getAccounts())[0];

// generating a token with 1 day of expiration time
const token = await Web3Token.sign(msg => web3.eth.personal.sign(msg, address), '1d');

// attaching token to axios authorization header
axios.post('/registration', { name: 'Adam' }, {
  headers: {
    'Authorization': token,
  }
})

// checking how it finds me in backend's database
axios.get('/me', {
  headers: {
    'Authorization': token,
  }
})

```

***

## Tip

> [编程范式游记（5）- 修饰器模式](https://time.geekbang.org/column/article/2723)

### 概述

装饰器模式 是一种

### Python 的 Decorator

函数式实现与类实现

#### 类实现

```python

class myDecorator(object):
    def __init__(self, fn):
        print("inside myDecorator.__init__()")
        self.fn = fn

    def __call__(self):
        self.fn()
        print("inside myDecorator.__call__()")


@myDecorator
def aFunction():
    print("inside aFunction()")


print("Finished decorating aFunction()")

aFunction()

# 输出：
# inside myDecorator.__init__()
# Finished decorating aFunction()
# inside aFunction()
# inside myDecorator.__call__()
```

1. 一个是__init__()，这个方法是在我们给某个函数 decorate 时被调用，所以，需要有一个 fn 的参数，也就是被 decorate 的函数。
2. 一个是__call__()，这个方法是在我们调用被 decorate 的函数时被调用的。

#### 函数实现

```python

from functools import wraps


def memoization(fn):
    cache = {}
    miss = object()

    @wraps(fn)
    def wrapper(*args):
        result = cache.get(args, miss)
        if result is miss:
            result = fn(*args)
            cache[args] = result
        return result

    return wrapper


@memoization
def fib(n):
    if n < 2:
        return n
    return fib(n - 1) + fib(n - 2)
```

#### 复杂示例

```python


class MyApp():
    def __init__(self):
        self.func_map = {}

    def register(self, name):
        def func_wrapper(func):
            self.func_map[name] = func
            return func

        return func_wrapper

    def call_method(self, name=None):
        func = self.func_map.get(name, None)
        if func is None:
            raise Exception("No function registered against - " + str(name))
        return func()


app = MyApp()


@app.register('/')
def main_page_func():
    return "This is the main page."


@app.register('/next_page')
def next_page_func():
    return "This is the next page."


print(app.call_method('/'))
print(app.call_method('/next_page'))
```

***

## Share

> [Dapr Actor](https://github.com/Carmenliukang/ARTS/blob/master/week43.md#share)

### 概述

actor 模式 阐述了 Actors 为最低级别的“计算单元”。 换句话说，您将代码写入独立单元 ( 称为actor) ，该单元接收消息并一次处理消息，而不进行任何类型的并行或线程处理。

当代码处理一条消息时，它可以向其他参与者发送一条或多条消息，或者创建新的 Actors。 底层 运行时 将管理每个 actor 的运行方式，时机和位置，并在 Actors 之间传递消息。

大量 Actors 可以同时执行，而 Actors 可以相互独立执行。

Dapr 包含专门实现 virtual actors 模式 的运行时。 通过 Dapr 的实现，您可以根据 Actors 模型编写 Dapr Actor，而 Dapr 利用底层平台提供的可扩展性和可靠性保证。

### 何时使用 Actors？

与任何其他技术决策一样，您应该根据您尝试解决的问题来决定是否使用 Actors。

Actor 设计模式可以很好适应一些分布式系统问题和场景，但您首先应该考虑的是模式的约束。 一般来说，在下列情况下，考虑 actor 模式来模拟你的问题或场景：

* 您的问题空间涉及大量(数千或更多) 的独立和孤立的小单位和逻辑。
* 您想要处理单线程对象，这些对象不需要外部组件的大量交互，例如在一组 Actors 之间查询状态。
* 您的 actor 实例不会通过发出I/O操作来阻塞调用方。

#### Dapr 中的 Actors

每个 actor 都定义为 actor 类型的实例，与对象是类的实例的方式相同。 例如，可能存在实现计算器功能的 actor 类型，并且该类型的许多 Actors 分布在集群的各个节点上。 每个这样的 actor 都是由一个 actor
ID 确定的。

![](https://github.com/Carmenliukang/ARTS/blob/master/image/43/5.png)

#### Actor 生命周期

Dapr Actors 是虚拟的，意思是他们的生命周期与他们的 in - memory 表现不相关。 因此，它们不需要显式创建或销毁。 Dapr Actors 运行时在第一次接收到该 actor ID 的请求时自动激活 actor。 如果
actor 在一段时间内未被使用，那么 Dapr Actors 运行时将回收内存对象。 如果以后需要重新启动，它还将保持对 actor 的一切原有数据。

调用 actor 方法和 reminders 将重置空闲时间，例如，reminders 触发将使 actor 保持活动状态。 不论 actor 是否处于活动状态或不活动状态 Actor reminders 都会触发，对不活动 actor
，那么会首先激活 actor。 Actor timers 不会重置空闲时间，因此 timer 触发不会使参与者保持活动状态。 Timer 仅在 actor 活跃时被触发。

空闲超时和扫描时间间隔 Dapr 运行时用于查看是否可以对 actor 进行垃圾收集。 当 Dapr 运行时调用 actor 服务以获取受支持的 actor 类型时，可以传递此信息。

Virtual actors 生命周期抽象会将一些警告作为 virtual actors 模型的结果，而事实上， Dapr Actors 实施有时会偏离此模型。

在第一次将消息发送到其 actor 标识时，将自动激活 actor ( 导致构造 actor 对象) 。 在一段时间后，actor 对象将被垃圾回收。 以后，再次使用 actor ID 访问，将构造新的 actor。 Actor
的状态比对象的生命周期更久，因为状态存储在 Dapr 运行时的配置状态提供程序中（也就是说Actor即使不在活跃状态，仍然可以读取它的状态）。

### 分发和故障转移

为了提供可扩展性和可靠性，Actors 实例分布在整个集群中， Dapr 会根据需要自动将对象从失败的节点迁移到健康的节点。

Actors 分布在 actor 服务的实例中，并且这些实例分布在集群中的节点之间。 每个服务实例都包含给定 Actors 类型的一组 Actors。

#### Actor 安置服务 (Actor placement service)

Dapr actor 运行时为您管理分发方案和键范围设置。 这是由 actor Placement 服务完成的。 创建服务的新实例时，相应的 Dapr 运行时将注册它可以创建的 actor 类型， Placement 服务将计算给定
actor 类型的所有实例之间的分区。 每个 actor 类型的分区信息表将更新并存储在环境中运行的每个 Dapr 实例中，并且可以随着新 actor 服务实例创建和销毁动态更改。 如下图所示。

![](https://github.com/Carmenliukang/ARTS/blob/master/image/43/6.png)

当客户端调用具有特定标识的 actor ( 例如，actor Id 123) 时，客户端的 Dapr 实例将散列 actor 类型和 Id，并使用该信息来调用相应的 Dapr 实例，该实例可以为该特定 actor Id提供请求。
因此，始终对任何给定 actor Id 始终会落在同一分区 (或服务实例) 。 如下图所示。

![](https://github.com/Carmenliukang/ARTS/blob/master/image/43/7.png)

这简化了一些选择，但也带有一些考虑：

* 默认情况下，Actors 被随机放入分区中，从而形成均匀的分布。
* 由于 Actors 是随机放置的，因此可知，执行操作始终需要网络通信，包括方法调用数据的序列化和去序列化，产生延迟和开销。

注: Dapr actor Placement 服务仅用于 actor 安置，因此，如果您的服务未使用 Dapr Actors，那么不需要。 The Placement service can run in all hosting
environments, including self-hosted and Kubernetes.

### Actor 通信

您可以通过 HTTP/gRPC 来与 Dapr 交互以调用 actor 方法.

```shell
POST/GET/PUT/DELETE http://localhost:3500/v1.0/actors/<actorType>/<actorId>/<method/state/timers/reminders>

```

您可以在请求主体中为 actor 方法提供任何数据，并且请求的响应在响应主体中，这是来自 actor 方法调用的数据。

#### 并发（Concurrency）

Dapr Actors 运行时提供了一个简单的基于回合的访问模型，用于访问 Actors 方法。 这意味着任何时候都不能有一个以上的线程在一个 actor 对象的代码内活动。 基于回合的访问大大简化了并发系统，因为不需要同步数据访问机制。
这也意味着系统的设计必须考虑到每个 actor 实例的单线程访问性质。

单个 actor 实例一次无法处理多个请求。 如果 actor 实例预期要处理并发请求，可能会导致吞吐量瓶颈。

如果两个 Actors 之间存在循环请求，而外部请求同时向其中一个 Actors 发出外部请求，那么 Actors 可以相互死锁。 Dapr actor 运行时会自动分出 actor 调用，并向调用方引发异常以中断可能死锁的情况。

![](https://github.com/Carmenliukang/ARTS/blob/master/image/43/8.png)

#### 基于回合的访问

一个回合包括执行 actor 方法以响应来自其他 Actors 或客户端的请求，或执行 timer/reminders 回调。 即使这些方法和回调是异步的，但 Dapr Actors 运行时并没有将它们交错（Interleave
，即并发调用它们）。 在允许新回合之前，必须完全结束之前的回合。 换句话说，在允许对方法或回调进行新调用之前，必须完全完成当前正在执行的 actor 方法或 timer/reminders 回调。
如果执行从方法或回调返回结果，并且方法或回调返回的任务已完成，则方法或回调将被视为已完成。 值得强调的是，即使在不同方法、timer和回调中，基于回合的并发也一样起作用。

Dapr Actors 运行时通过在回合开始时获取每个 Actors 的锁定并在该回合结束时释放锁定来实施基于回合的并行。 因此，基于回合的并发性是按每个 actor 执行的，而不是跨 Actors 执行的。 Actor 方法和
timer/reminders 回调可以代表不同的 Actors 同时执行。

下面的示例演示了上述概念。 现在有一个实现了两个异步方法（例如，方法 1 和方法 2）、timer 和 reminders 的 actor。 下图显示了执行这些方法的时间线的示例，并代表属于此 Actors 类型的两个 Actors (
ActorId1 和 ActorId2) 的回调。

![](https://github.com/Carmenliukang/ARTS/blob/master/image/43/9.png)


