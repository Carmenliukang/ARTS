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
>[7 Terminal Commands That Will Just Make You Smile](https://medium.com/better-programming/7-terminal-commands-that-will-just-make-you-smile-3f5bc8778080)


### 概述
介绍了一些 linux 趣味命令行
1. sl 

   

2. fortune

    ![](https://miro.medium.com/max/1080/0*jWCXvQd_RbS94X-6.png)

3. cowsay

    ![](https://miro.medium.com/max/560/0*fwo-m6neieB0pFFT.png)

4. toilet
    
    ![](https://miro.medium.com/max/762/0*xJChkPXgM0ZjeACk.png)

5. oneko

    ![](https://youtu.be/Nm3SkXThL0s)
    
6. xeyes

7. cmatrix


### 思考
工作中的一些小趣事，也是能够提高一定的工作效率。生活处处是惊喜，不论如何，都要不断调整，让生活更加美好才可以。

Markdown 里面的 GIF 增加晚上补上。

***
## Tip
>[操作系统内核与编程接口](https://time.geekbang.org/column/article/94486)

### 概述
**Linux内核**

分为用户态、内核态。只有进程被系统调度到内核态才能够访问内存，在用户态的时候，是不能访问到真实的内存数据，只能获取到虚拟内存中的数据。
因此才会有协程的存在，避免了因为上下文切换导致的系统问题。
![](https://static001.geekbang.org/resource/image/2b/b3/2b0adde3eca6262ae674a97f478c15b3.png)


**动态库**：屏蔽底层操作，只保留最基本的操作
* Windows 的 dll（Dynamic Link Library）；
* Linux/Android 的 so（shared object）；
* Mac/iOS 的 dylib（Mach-O Dynamic Library）。
![](https://static001.geekbang.org/resource/image/37/11/372f60e314a3ec386844d4cd1db74411.jpg)

**系统架构**
![](https://static001.geekbang.org/resource/image/b2/e0/b2393a109f849bd91c991b1e750cb3e0.png)

### 思考

Linux系统通过虚拟内存实现了安全性的保证，同时通过 Ring 0-3 实现了权限控制。
现在的虚拟化技术，也是在linux系统的扩展，实现了更多的功能，以后的开发一定会越来越高效，新技术也会越来越多，不过更多的是从底层发展而来，就如docker和Linux系统中的虚拟内存是否有异曲同工？
那么只有底层能够更加深入的了解，那么才能以不变应万变，知其然，同时知其所以然，才不会被时代抛弃。
浅尝辄止，只会皮毛，那么最终一定会被时代抛弃。

***
## Share
>[多任务：进程、线程与协程](https://time.geekbang.org/column/article/96324)

### 概述
进程、线程 的成本在 执行体 的开销，包括本身和切换。
由此而优化，那么就是降低 线程 数量，由此就有了异步的状态，epoll 去循环遍历，监控线程进度。
那么异步的回调，就会陷入回调地狱，没有办法掌控其和调度，因此有了协程的存在。在用户态中调度，降低了切换的开销成本。

### 思考
用户态中避免了系统调度，但是以后的 CPU 性能越来越高，这种成本是否可以忽略不计？那么协程存在的意义又是什么？
协程是为了能够更加轻便的实现系统调度方式。更加轻松的应对IO问题。



