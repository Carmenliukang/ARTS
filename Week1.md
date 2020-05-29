# ARTS 2020-05-28 week 1

***

## Algoithm

> [字符串解码](https://leetcode-cn.com/problems/decode-string/)

    给定一个经过编码的字符串，返回它解码后的字符串。
    
    编码规则为: k[encoded_string]，表示其中方括号内部的 encoded_string 正好重复 k 次。注意 k 保证为正整数。
    
    你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。
    
    此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 k ，例如不会出现像 3a 或 2[4] 的输入。
    
    示例:
    
    s = "3[a]2[bc]", 返回 "aaabcbc".
    s = "3[a2[c]]", 返回 "accaccacc".
    s = "2[abc]3[cd]ef", 返回 "abcabccdcdcdef".


### 解题思路
    使用 **栈** 思路的方式
    
    本题核心思路是在栈里面每次存储两个信息, 
    (左括号前的字符串, 左括号前的数字), 
    比如abc3[def], 当遇到第一个左括号的时候，压入栈中的是("abc", 3), 
    然后遍历括号里面的字符串def, 当遇到右括号的时候, 从栈里面弹出一个元素(s1, n1), 
    得到新的字符串为s1+n1*"def", 也就是abcdefdefdef。对于括号里面嵌套的情况也是同样处理方式。
    凡是遇到左括号就进行压栈处理，遇到右括号就弹出栈，栈中记录的元素很重要。
    

#### 代码
```python
class Solution:
    def decodeString(self, s: str) -> str:
        stack = []  # (str, int) 记录左括号之前的字符串和左括号外的上一个数字
        num = 0
        res = ""  # 实时记录当前可以提取出来的字符串
        for c in s:
            if c.isdigit():
                num = num * 10 + int(c)
            elif c == "[":
                stack.append((res, num))
                res, num = "", 0
            elif c == "]":
                top = stack.pop()
                res = top[0] + res * top[1]
            else:
                res += c
        return res
```
***
## Review 
> [Data Transformation Using the Decorator Pattern](https://levelup.gitconnected.com/data-transformation-using-the-decorator-pattern-d94441d13b66)

    * 自身英语基础能力非常差，阅读确实是非常吃力的，一边翻译一边对比，确实也对于学习英语防范也是有很大的帮助和提升的。

### 概要
    * 这篇文档，主要介绍了通过装饰器模式以最小的成本去实现业务需求的变更，具体使用的是 通过 interface or class 方式进行方法的覆盖。达到这种结果，
    * 同时对比Python 的装饰器，这种装饰器是通过多态/继承 方式实现，相比较 Python 的装饰器，不会修改其作用域，对于代码层面的影响较小，但是后期不易维护，
    * 如果多次修改，那么最后的结果可能就是多个装饰器嵌套，不符合 单一性原则 同时也不符合 高内聚低耦合 的系统架构设计原则。
    * 最后总结，系统设置方面还是需要尽可能的 为后期的字段增加/业务逻辑变化 有更加高的开放性 ，保证能够快速实现。

***
## Tip
> [mysql sql 执行慢的原因和处理方案](https://time.geekbang.org/column/article/74687)

### 概述
    1. 锁
    
    |类型|解决办法|其他说明|
    | :----: | :----:  | :----:  |
    |MDL 锁|查看 mysql 正在执行的 线程 ，然后 kill 相关锁住的 session| show processlist 找到 Waiting for table metadata lock |
    |flush|查看 mysql 正在执行的 线程 ，然后 kill 相关 flush 的 session|有可能是因为 flush 的时候，有相关的 sql select 查询|
    |等行锁| 通过 sys.innodb_lock_waits 这个表查看，哪些行级锁 | select * from t sys.innodb_lock_waits where locked_table='`test`.`t`'\G |
    |有无索引| 如果没有索引，可能会全表扫描|坏查询不一定是慢查询，需要考虑，如果数据量很大，那么也需要这样控制|
    | 一行却很慢 |一致性读的|因为有存在 开始 事务 后，还会有 update 其他字段的情况，所以在一致读/可重复读/读已提交/读未提交 这种情况下，需要进行读写分离，降低字段的 update 操作。|
    ***

***
## Share
> [server 增加 queue 提升性能](https://xie.infoq.cn/article/8f0179960545d0791a2e537e9)

    这部分主要是，在server 层面增加一层队列来应对，请求突升这种情况，是一种饮鸠止渴的应对方式，同时正确的应该是能够优化后端代码，
    同时实现自动扩容方式。

***