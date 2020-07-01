# ARTS Week 7
>![](https://s1.ax1x.com/2020/07/01/NTik7t.jpg)
>> 愿更加坚强 愿自己能够更加坚强

***
## Algoithm
> [打开转盘锁](https://leetcode-cn.com/problems/open-the-lock/)

### 概述
你有一个带有四个圆形拨轮的转盘锁。每个拨轮都有10个数字： '0', '1', '2', '3', '4', '5', '6', '7', '8', '9' 。每个拨轮可以自由旋转：例如把 '9' 变为  '0'，'0' 变为 '9' 。每次旋转都只能旋转一个拨轮的一位数字。

锁的初始数字为 '0000' ，一个代表四个拨轮的数字的字符串。

列表 deadends 包含了一组死亡数字，一旦拨轮的数字和列表里的任何一个元素相同，这个锁将会被永久锁定，无法再被旋转。

字符串 target 代表可以解锁的数字，你需要给出最小的旋转次数，如果无论如何不能解锁，返回 -1。

 
```
示例 1:

输入：deadends = ["0201","0101","0102","1212","2002"], target = "0202"
输出：6
解释：
可能的移动序列为 "0000" -> "1000" -> "1100" -> "1200" -> "1201" -> "1202" -> "0202"。
注意 "0000" -> "0001" -> "0002" -> "0102" -> "0202" 这样的序列是不能解锁的，
因为当拨动到 "0102" 时这个锁就会被锁定。
示例 2:

输入: deadends = ["8888"], target = "0009"
输出：1
解释：
把最后一位反向旋转一次即可 "0000" -> "0009"。
示例 3:

输入: deadends = ["8887","8889","8878","8898","8788","8988","7888","9888"], target = "8888"
输出：-1
解释：
无法旋转到目标数字且不被锁定。
示例 4:

输入: deadends = ["0000"], target = "8888"
输出：-1
```
 

提示：

死亡列表 deadends 的长度范围为 [1, 500]。
目标数字 target 不会在 deadends 之中。
每个 deadends 和 target 中的字符串的数字会在 10,000 个可能的情况 '0000' 到 '9999' 中产生。



### 分析
最值问题 都可以使用 DFS/BFS 同步，可以按照如下的模板进行尝试


### 代码
```python3

# Python 的内部队列，可以双进双出
from collections import deque


class Solution:
    """
    1. 这里使用 图论 的方式去解开这个问题，每一次都只能向上、向下 进行旋转。
    2. 所需要防止走回头路
    3. 需要过滤 中间死亡节点

    """

    def openLock(self, deadends, target):
        # 初始队列
        q = deque()

        # 加入初始元素
        q.append("0000")
        # 记录死亡数字，直接过滤
        deadends_dict = {i: True for i in deadends}
        # 防止回头，产生死循环
        visited_dict = {}
        step = 0

        # 开始BFS遍历
        while q:
            for i in range(len(q)):
                # 这里是每次从最左侧取出数据
                cur = q.popleft()
                # 过滤死亡数字
                if deadends_dict.get(cur):
                    continue
                # 如果结果一致，那么返回步数
                if cur == target:
                    return step

                # 因为锁只有四个位置
                for k in range(4):
                    # 向上调整一位
                    up = self.plusOne(cur, k)
                    # 防止死循环，过滤数字
                    if not visited_dict.get(up):
                        q.append(up)
                        visited_dict[up] = True

                    # 向下调整一位
                    down = self.minusOne(cur, k)
                    # 防止死循环，过滤数字
                    if not visited_dict.get(down):
                        q.append(down)
                        visited_dict[down] = True

            step += 1
        return -1

    # 向上增加一位
    def plusOne(self, s: str, j: int) -> str:
        ch = list(s)
        if ch[j] == "9":
            ch[j] = "0"
        else:
            ch[j] = str(int(ch[j]) + 1)
        return "".join(ch)

    # 向下减少一位
    def minusOne(self, s: str, j: int) -> str:
        ch = list(s)
        if ch[j] == "0":
            ch[j] = "9"
        else:
            ch[j] = str(int(ch[j]) - 1)

        return "".join(ch)

```

### 总结
BFS 模板
```java

// 计算从起点 start 到终点 target 的最近距离
int BFS(Node start, Node target) {
    Queue<Node> q; // 核心数据结构
    Set<Node> visited; // 避免走回头路

    q.offer(start); // 将起点加入队列
    visited.add(start);
    int step = 0; // 记录扩散的步数

    while (q not empty) {
        int sz = q.size();
        /* 将当前队列中的所有节点向四周扩散 */
        for (int i = 0; i < sz; i++) {
            Node cur = q.poll();
            /* 划重点：这里判断是否到达终点 */
            if (cur is target)
                return step;
            /* 将 cur 的相邻节点加入队列 */
            for (Node x : cur.adj())
                if (x not in visited) {
                    q.offer(x);
                    visited.add(x);
                }
        }
        /* 划重点：更新步数在这里 */
        step++;
    }
}
```

***
## Review
>[TODO]()


### 概述


### 思考


***
## Tip
>[TODO]()

### 概述

### 思考


***
## Share
>[TODO]()

### 概述

### 思考


