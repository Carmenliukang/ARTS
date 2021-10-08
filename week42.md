# ARTS Week 42

> ![](https://github.com/Carmenliukang/ARTS/blob/master/image/42/erik-mclean-Rt08GhMIf3g-unsplash.jpg)
>> 从零开始

***

## Algoithm

> [最接近的二叉搜索树值 II](https://leetcode-cn.com/problems/closest-binary-search-tree-value-ii/)

### 概述

给定一个不为空的二叉搜索树和一个目标值 target，请在该二叉搜索树中找到最接近目标值 target 的 k 个值。

注意：

* 给定的目标值 target 是一个浮点数
* 你可以默认 k 值永远是有效的，即 k ≤ 总结点数
* 题目保证该二叉搜索树中只会存在一种 k 个值集合最接近目标值

示例：

    输入: root = [4,2,5,1,3]，目标值 = 3.714286，且 k = 2

        4
       / \
      2   5
     / \
    1   3

    输出: [4,3]

拓展：

假设该二叉搜索树是平衡的，请问您是否能在小于 O(n)（n 为总结点数）的时间复杂度内解决该问题呢？

### 分析

二叉搜索树 中序遍历 为从小到大的结果

### code

```python

from typing import List


# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:
    def __init__(self):
        self.nums = []

    def closestKValues(self, root: TreeNode, target: float, k: int) -> List[int]:
        self.dfs(root)
        result = self.check(target)
        return [self.nums[i[0]] for i in result[:k]]

    def dfs(self, root: TreeNode):
        if root is None:
            return
        self.dfs(root.left)
        self.nums.append(root.val)
        self.dfs(root.right)

    def check(self, target: float) -> List[int]:
        result_map = {}
        for index, num in enumerate(self.nums):
            result_map[index] = abs(target - num)

        data = sorted(result_map.items(), key=lambda item: item[1])
        return data
```

***

## Review

> [You Should Start Using FastAPI Now](https://towardsdatascience.com/you-should-start-using-fastapi-now-7efb280fec02)

### 概述

**[FastAPI](https://fastapi.tiangolo.com/)** is a relatively new web framework for Python, taking inspiration from its
predecessors, perfecting them and fixing many of their flaws. Built on top of Starlette, it brings a ton of awesome
features to the table.

#### Simple, yet brilliant interface

FastAPI is more on the Flask side of the spectrum, but it manages to strike a healthier balance

```python
from fastapi import FastAPI
from pydantic import BaseModel


class User(BaseModel):
    email: str
    password: str


app = FastAPI()


@app.post("/login")
def login(user: User):
    # ...
    # do some magic
    # ...
    return {"msg": "login successful"}

```

For defining the schema, it uses **Pydantic**, which is another awesome Python library, used for data validation. This
is simple to do here, yet so much is happening in the background. The responsibility to validate the input is delegated
to FastAPI. If the request is not right, for instance the email field contains an int, an appropriate error code will be
returned, instead of the app breaking down with the dreaded Internal Server Error (500). And it is practically free.

This simple example app can be served with uvicorn:

```shell
uvicorn main:app

```

The icing on the cake is that it automatically generates the documentation according to the OpenAPI using the
interactive Swagger UI.

![](https://github.com/Carmenliukang/ARTS/blob/master/image/42/1.png)

#### Async

```python

from fastapi import FastAPI

app = FastAPI()


@app.post("/")
async def endpoint():
    # ...
    # call async functions here with `await`
    # ...
    return {"msg": "FastAPI is awesome!"}
```

#### Dependency injections

FastAPI has a really cool way to manage dependencies. Although it is not forced on the developer, it is strongly
encouraged to use the built-in injection system to handle dependencies in your endpoints.

```python
from fastapi import FastAPI, Depends
from pydantic import BaseModel


class Comment(BaseModel):
    username: str
    content: str


app = FastAPI()

database = {
    "articles": {
        1: {
            "title": "Top 3 Reasons to Start Using FastAPI Now",
            "comments": []
        }
    }
}


def get_database():
    return database


@app.post("/articles/{article_id}/comments")
def post_comment(article_id: int, comment: Comment, database=Depends(get_database)):
    database["articles"][article_id]["comments"].append(comment)
    return {"msg": "comment posted!"}
```

#### Easy integration with databases

```python
from fastapi import FastAPI, Depends

from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

engine = create_engine("sqlite:///./database.db")
Session = sessionmaker(bind=engine)


def get_db():
    return Session()


app = FastAPI()


@app.get("/")
def an_endpoint_using_sql(db=Depends(get_db)):
    # ...
    # do some SQLAlchemy
    # ...
    return {"msg": "an exceptionally successful operation!"}
```

#### GraphQL support

When you are working with a complex data model, REST can be a serious hindrance. It is definitely not fun when a tiny
change in the frontend requires to update the schema for an endpoint. GraphQL shines in these situations. Although
GraphQL support is not unique among Python web frameworks, Graphene and FastAPI work together seamlessly. No need to
install any extensions like graphene_django for Django, it just works natively.


***

## Tip

> [编程范式游记（3） - 类型系统和泛型的本质](https://time.geekbang.org/column/article/2017)

### 概述

在编程世界中，我们需要做好两件事情：

1. 编程语言中的类型问题。
2. 真实世界中的业务代码的抽象、重用、拼装。

#### 类型系统

一般分成两种：

1. int、float、char
2. struct class function

抽象类型在程序运行中，可能不表示为值。类型系统在各种语言之间有非常大的不同，也许，最主要的差异存在于编译时期的语法，以及运行时期的操作实现方式。

类型系统的主要功能：

1. 程序语言的安全性
2. 利于编译器的优化
3. 代码的可读性
4. 抽象画

**类型带来的问题就是我们作用于不同类型的代码，虽然长得非常相似，但是由于类型的问题需要根据不同版本写出不同的算法，如果要做到泛型，就需要涉及比较底层的玩法。**

因此世界有两种语言：

1. 静态语言: C/C++/Java
2. 动态语言: Python/PHP/JavaScript

动态语言示例：

```python
x = 5;
x = "hello";
```

在弱类型或是动态类型的语言中，下面代码的执行会有不确定的结果。

```shell

x = 5;
y = "37";
z = x + y;

```

* 有的像 Visual Basic 语言，给出的结果是 42：系统将字符串 "37" 转换成数字 37，以匹配运算上的直觉。
* 而有的像 JavaScript 语言，给出的结果是 "537"：系统将数字 5 转换成字符串 "5" 并把两者串接起来。
* 像 Python 这样的语言，则会产生一个运行时错误。

**我们需要清楚地知道，无论哪种程序语言，都避免不了一个特定的类型系统。**

1. 静态类型检查是在编译器进行语义分析时进行的。如果一个语言强制实行类型规则（即通常只允许以不丢失信息为前提的自动类型转换），那么称此处理为强类型，反之称为弱类型。
2. 动态类型检查系统更多的是在运行时期做动态类型标记和相关检查。所以，动态类型的语言必然要给出一堆诸如：is_array(), is_int(), is_string() 或是 typeof() 这样的运行时类型检查函数。

### 泛型的本质

类型的本质：

* 类型是对内存的一种抽象。不同的类型，会有不同的内存布局和内存分配的策略。
* 不同的类型，有不同的操作。所以，对于特定的类型，也有特定的一组操作。

要做到泛型，我们需要做下面的事情：

* 标准化掉类型的内存分配、释放和访问。
* 标准化掉类型的操作。比如：比较操作，I/O 操作，复制操作……
* 标准化掉数据容器的操作。比如：查找算法、过滤算法、聚合算法……
* 标准化掉类型上特有的操作。需要有标准化的接口来回调不同类型的具体操作……

#### C++ 的实现方法

C++ 动用了非常繁多和复杂的技术来达到泛型编程的目标

* 通过类中的构造、析构、拷贝构造，重载赋值操作符，标准化（隐藏）了类型的内存分配、释放和复制的操作。
* 通过重载操作符，可以标准化类型的比较等操作。
* 通过 iostream，标准化了类型的输入、输出控制。
* 通过模板技术（包括模板的特化），来为不同的类型生成类型专属的代码。
* 通过迭代器来标准化数据容器的遍历操作。
* 通过面向对象的接口依赖（虚函数技术），来标准化了特定类型在特定算法上的操作。
* 通过函数式（函数对象），来标准化对于不同类型的特定操作。

#### 泛型编程[Generic Programming](http://stepanovpapers.com/genprog.pdf)

> Generic programming centers around the idea of abstracting from concrete, efficient algorithms to obtain generic algorithms that can be combined with different data representations to produce a wide variety of useful software.— Musser, David R.; Stepanov, Alexander A., Generic Programming

屏蔽掉数据和操作数据的细节，让算法更为通用，让编程者更多地关注算法的结构，而不是在算法中处理不同的数据类型。

### 小结

在编程语言中，类型系统的出现主要是对容许混乱的操作加上了严格的限制，以避免代码以无效的数据使用方式编译或运行。例如，整数运算不可用于字符串；指针的操作不可用于整数上，等等。但是，类型的产生和限制，虽然对底层代码来说是安全的，但是对于更高层次的抽象产生了些负面因素。比如在
C++ 语言里，为了同时满足静态类型和抽象，就导致了模板技术的出现，带来了语言的复杂性。

我们需要清楚地明白，编程语言本质上帮助程序员屏蔽底层机器代码的实现，而让我们可以更为关注于业务逻辑代码。但是因为，编程语言作为机器代码和业务逻辑的粘合层，是在让程序员可以控制更多底层的灵活性，还是屏蔽底层细节，让程序员可以更多地关注于业务逻辑，这是很难两全需要
trade-off 的事。

所以，不同的语言在设计上都会做相应的取舍，比如：C 语言偏向于让程序员可以控制更多的底层细节，而 Java 和 Python 则让程序员更多地关注业务功能的实现。而 C++ 则是两者都想要，导致语言在设计上非常复杂。


***

## Share

> [Dapr Pub/Sub](https://github.com/Carmenliukang/ARTS/blob/master/week42.md#share)

### 概述

**How-To: Publish a message and subscribe to a topic**

Pub/Sub is a common pattern in a distributed system with many services that want to utilize decoupled, asynchronous
messaging. Using Pub/Sub, you can enable scenarios where event consumers are decoupled from event producers.

Dapr provides an extensible Pub/Sub system with At-Least-Once guarantees, allowing developers to publish and subscribe
to topics. Dapr provides components for pub/sub, that enable operators to use their preferred infrastructure, for
example Redis Streams, Kafka, etc.

#### Content Types

When publishing a message, it’s important to specify the content type of the data being sent. Unless specified, Dapr
will assume text/plain. When using Dapr’s HTTP API, the content type can be set in a Content-Type header. gRPC clients
and SDKs have a dedicated content type parameter.

#### Step 1: Setup the Pub/Sub component

The following example creates applications to publish and subscribe to a topic called deathStarStatus.

![](https://github.com/Carmenliukang/ARTS/blob/master/image/42/2.png)

```yaml

apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: pubsub
spec:
  type: pubsub.redis
  version: v1
  metadata:
    - name: redisHost
      value: localhost:6379
    - name: redisPassword
      value: ""

```

#### Step 2: Subscribe to topics

Dapr allows two methods by which you can subscribe to topics:

* **Declaratively**, where subscriptions are defined in an external file.
* **Programmatically**, where subscriptions are defined in user code.

##### Declarative subscriptions

```yaml
apiVersion: dapr.io/v1alpha1
kind: Subscription
metadata:
  name: myevent-subscription
spec:
  topic: deathStarStatus
  route: /dsstatus
  pubsubname: pubsub
scopes:
  - app1
  - app2

```

##### Example

```python

import flask
from flask import request, jsonify
from flask_cors import CORS
import json
import sys

app = flask.Flask(__name__)
CORS(app)


@app.route('/dsstatus', methods=['POST'])
def ds_subscriber():
    print(request.json, flush=True)
    return json.dumps({'success': True}), 200, {'ContentType': 'application/json'}


app.run()

```

#### Step 3: Publish a topic

To publish a topic you need to run an instance of a Dapr sidecar to use the pubsub Redis component. You can use the
default Redis component installed into your local environment.

```shell
dapr run --app-id testpubsub --dapr-http-port 3500

# test 
dapr publish --publish-app-id testpubsub --pubsub pubsub --topic deathStarStatus --data '{"status": "completed"}'


```

#### Step 4: ACK-ing a message

```python

@app.route('/dsstatus', methods=['POST'])
def ds_subscriber():
    print(request.json, flush=True)
    return json.dumps({'success': True}), 200, {'ContentType': 'application/json'}

```

