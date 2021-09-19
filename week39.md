# ARTS Week 39

> ![](https://github.com/Carmenliukang/ARTS/blob/master/image/39/kristaps-ungurs-bqRUmr144Rc-unsplash.jpg)
>> 凡事过往，皆为序章

***

## Algoithm

> [从字符串生成二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-string)

### 概述

你需要从一个包括括号和整数的字符串构建一棵二叉树。

输入的字符串代表一棵二叉树。它包括整数和随后的 0 ，1 或 2 对括号。整数代表根的值，一对括号内表示同样结构的子树。

若存在左子结点，则从左子结点开始构建。

示例：

    输入："4(2(3)(1))(6(5))"
    输出：返回代表下列二叉树的根节点:
    
           4
         /   \
        2     6
       / \   /
      3   1 5

提示：

输入字符串中只包含'(', ')', '-'和'0' ~ '9'

空树由""而非"()"表示。

### 分析

二叉树的题目，总体按照先序/中序/后序 方式进行递归生成。

1. 首先判断终止条件：
    1. 如果为 "" ，则为 root
    2. 如果没有 "("，那么说明完整组成 root节点

2. 进行左右子树分割
    1. 按照格"()" 进行切割
    2. 如果组合为0，且开始节点保持一致，那么就说明是左子树，否则为右子树。

3. 递归生成

### code

```python


# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:
    def str2tree(self, s: str) -> TreeNode:
        root = self.dfs(s)
        return root

    def dfs(self, s):
        if len(s) == 0:
            return None
        if '(' not in s:
            return TreeNode(int(s[:]))
        pos = s.index('(')
        root = TreeNode(int(s[0: pos]))
        start = pos  # （start是某棵子树，区间的起始index，第一个左括号）
        cnt = 0
        for i in range(pos, len(s)):
            if s[i] == '(':
                cnt += 1
            elif s[i] == ')':
                cnt -= 1

            if cnt == 0 and start == pos:  # 左子的部分
                root.left = self.str2tree(s[start + 1: i])
                start = i + 1
            elif cnt == 0 and start != pos:  # 右子的部分 这个地方必须用 elif!!!!!!
                root.right = self.str2tree(s[start + 1: i])

        return root

```

***

## Review

> [[Tutorial, Part 1] How to develop Go gRPC microservice with HTTP/REST endpoint, middleware, Kubernetes deployment, etc.](https://medium.com/@amsokol.com/tutorial-how-to-develop-go-grpc-microservice-with-http-rest-endpoint-middleware-kubernetes-daebb36a97e9)

### 概述

I want to provide step by step tutorial how to develop simple CRUD “To Do list” microservice with gRPC and HTTP/REST
endpoints. I demonstrate how to write tests and add middleware (request-ID and logging/tracing) to microservice also. I
provide example how to build and deploy this microservice to Kubernetes at the end.

### Table of Content

1. about how to create gRPC CRUD service and client
2. about how to add HTTP/REST endpoint to the gRPC service
3. about how to add middleware (e.g. logging/tracing) to gRPC service and HTTP/REST endpoint as well
4. going to be dedicated how to add Kubernetes deployment configuration with health check and how to build and deploy
   project to Google Cloud

#### create gRPC CRUD service

1. https://github.com/amsokol/go-grpc-http-rest-microservice-tutorial/tree/part1
2. https://github.com/golang-standards/project-layout

***

## Tip

> [推荐阅读：分布式系统架构经典资料](https://time.geekbang.org/column/article/2080)

### 概述

分布式框架的相关资料汇总

### 基础概念

* CAP 定理
* Fallacies of Distributed Computing

### 经典资料

* Distributed systems theory for the distributed systems engineer
* FLP Impossibility Result
* An introduction to distributed systems
* Distributed Systems for fun and profit
* Distributed Systems: Principles and Paradigms
* Scalable Web Architecture and Distributed Systems
* Principles of Distributed Systems
* Making reliable distributed systems in the presence of software errors
* Designing Data Intensive Applications

### CAP 定理

CAP 定理是分布式系统设计中最基础，也是最为关键的理论。它指出，分布式数据存储不可能同时满足以下三个条件。

* 一致性（Consistency）：每次读取要么获得最近写入的数据，要么获得一个错误。
* 可用性（Availability）：每次请求都能获得一个（非错误）响应，但不保证返回的是最新写入的数据。
* 分区容忍（Partition tolerance）：尽管任意数量的消息被节点间的网络丢失（或延迟），系统仍继续运行。

![](https://github.com/Carmenliukang/ARTS/blob/master/image/39/1.webp)

#### 错误假设

1. 网络是稳定的。网络传输的延迟是零。
2. 网络的带宽是无穷大。网络是安全的。
3. 网络的拓扑不会改变。
4. 只有一个系统管理员。
5. 传输数据的成本为零。
6. 整个网络是同构的。

### 资料总结

[Distributed systems theory for the distributed systems engineer](https://www.the-paper-trail.org/post/2014-08-09-distributed-systems-theory-for-the-distributed-systems-engineer/)

[Distributed Systems for Fun and Profit](http://book.mixu.net/distsys/)
这是一本小书，涵盖了分布式系统中的关键问题，包括时间的作用和不同的复制策略。后文中对这本书有较详细的介绍。

[Notes on distributed systems for young bloods](https://www.somethingsimilar.com/2013/01/14/notes-on-distributed-systems-for-young-bloods/)
，这篇文章中没有理论，是一份适合新手阅读的分布式系统实践笔记。

[A Note on Distributed Systems](http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.41.7628)
，这是一篇经典的论文，讲述了为什么在分布式系统中，远程交互不能像本地对象那样进行。

[The fallacies of distributed computing](https://en.wikipedia.org/wiki/Fallacies_of_distributed_computing)
，每个分布式系统新手都会做的 8 个错误假设，并探讨了其会带来的影响。上文中专门对这篇文章做了介绍。


***

## Share

> [ORM简单的模板-Python](https://github.com/Carmenliukang/ARTS/blob/master/week39.md#share)

### 概述

整理了部分基于 ORM框架的同步/异步 相关组建。

### PSql

```python

import os
import asyncio
import inspect
import functools
import collections

import orjson
from blinker import signal
from sqlalchemy import exc, event, create_engine
from sqlalchemy.engine.base import Engine as SQLAlchemyEngine
from sqlalchemy.orm import UOWTransaction, sessionmaker, scoped_session
from sqlalchemy.pool import NullPool
from sqlalchemy.ext.asyncio import AsyncSession as _AsyncSession
from sqlalchemy.ext.asyncio import create_async_engine, async_scoped_session

# 相关的设置
from settings import DB_DSN, DB_POOL_SIZE, DB_MAX_OVERFLOW, DB_POOL_RECYCLE

engine: SQLAlchemyEngine = create_engine(
    DB_DSN,
    pool_size=DB_POOL_SIZE,
    max_overflow=DB_MAX_OVERFLOW,
    pool_recycle=DB_POOL_RECYCLE,
    json_serializer=lambda obj: orjson.dumps(obj).decode(),
    json_deserializer=lambda obj: orjson.loads(obj),
)

# Constructs a scoped DBSession.
Session = scoped_session(
    sessionmaker(engine, expire_on_commit=False, future=True),
)


@event.listens_for(engine, "connect")
def connect(dbapi_connection, connection_record):
    connection_record.info["pid"] = os.getpid()


@event.listens_for(engine, "checkout")
def checkout(dbapi_connection, connection_record, connection_proxy):
    pid = os.getpid()
    if connection_record.info["pid"] != pid:
        connection_record.connection = connection_proxy.connection = None
        raise exc.DisconnectionError(
            "Connection record belongs to pid %s, "
            "attempting to check out in pid %s" % (connection_record.info["pid"], pid)
        )


@event.listens_for(Session, "after_flush")
def after_flush(session, flush_context: UOWTransaction):
    if not hasattr(session, "g_changed_track_info"):
        session.g_changed_track_info = collections.defaultdict(set)

    for instances in flush_context.mappers.values():
        for instance in instances:
            session.g_changed_track_info[instance.class_.__name__].add(
                instance.identity
            )


@event.listens_for(Session, "after_commit")
def after_commit(session):
    if not hasattr(session, "g_changed_track_info"):
        return

    for model_name, primary_keys in session.g_changed_track_info.items():
        signal(model_name).send(primary_keys)
    delattr(session, "g_changed_track_info")


async_engine = create_async_engine(
    DB_DSN.replace("psycopg2", "asyncpg"),
    json_serializer=lambda obj: orjson.dumps(obj).decode(),
    json_deserializer=lambda obj: orjson.loads(obj),
    poolclass=NullPool,
)

_session_factory = sessionmaker(
    async_engine, expire_on_commit=False, class_=_AsyncSession
)


def _hooked_session_maker():
    async_session = _session_factory()

    @event.listens_for(async_session.sync_session, "after_flush")
    def _after_flush(session, flush_context: UOWTransaction):
        if not hasattr(session, "g_changed_track_info"):
            session.g_changed_track_info = collections.defaultdict(set)

        for instances in flush_context.mappers.values():
            for instance in instances:
                session.g_changed_track_info[instance.class_.__name__].add(
                    instance.identity
                )

    @event.listens_for(async_session.sync_session, "after_commit")
    def _after_commit(session):
        if not hasattr(session, "g_changed_track_info"):
            return

        for model_name, primary_keys in session.g_changed_track_info.items():
            signal(model_name).send(primary_keys)
        delattr(session, "g_changed_track_info")

    return async_session


AsyncSession = async_scoped_session(
    _hooked_session_maker,
    scopefunc=asyncio.current_task,
)


def session_scope(fn):
    """Defines session top level life time, session will be closed after scope ends."""

    if inspect.iscoroutinefunction(fn):

        @functools.wraps(fn)
        async def wrapper_decorator(*args, **kwargs):
            async with AsyncSession():
                return await fn(*args, **kwargs)

    else:

        @functools.wraps(fn)
        def wrapper_decorator(*args, **kwargs):
            with Session():
                return fn(*args, **kwargs)

    return wrapper_decorator


def commit_scope(fn):
    """Defines transaction top level life time, force begin /close transaction."""

    if inspect.iscoroutinefunction(fn):

        @functools.wraps(fn)
        async def wrapper_decorator(*args, **kwargs):
            session = AsyncSession()
            try:
                res = await fn(*args, **kwargs)
                await session.commit()
                return res

            except Exception:
                await session.rollback()
                raise

    else:

        @functools.wraps(fn)
        def wrapper_decorator(*args, **kwargs):
            session = Session()
            try:
                res = fn(*args, **kwargs)
                session.commit()
                return res
            except Exception:
                session.rollback()
                raise

    return wrapper_decorator

```
