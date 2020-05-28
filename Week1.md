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

***