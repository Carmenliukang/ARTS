# ARTS Week 32

> ![](https://github.com/Carmenliukang/ARTS/blob/master/image/32/1.webp)
>> 当你走出家乡的时候，你就是你自己的家乡

***

## Algoithm

> [二叉树的边界](https://leetcode-cn.com/problems/boundary-of-binary-tree/)

### 概述

二叉树的 边界 是由 根节点 、左边界 、按从左到右顺序的 叶节点 和 逆序的右边界 ，按顺序依次连接组成。

左边界 是满足下述定义的节点集合：

根节点的左子节点在左边界中。如果根节点不含左子节点，那么左边界就为 空 。 如果一个节点在左边界中，并且该节点有左子节点，那么它的左子节点也在左边界中。 如果一个节点在左边界中，并且该节点 不含 左子节点，那么它的右子节点就在左边界中。
最左侧的叶节点 不在 左边界中。 右边界 定义方式与 左边界 相同，只是将左替换成右。即，右边界是根节点右子树的右侧部分；叶节点 不是 右边界的组成部分；如果根节点不含右子节点，那么右边界为 空 。

叶节点 是没有任何子节点的节点。对于此问题，根节点 不是 叶节点。

给你一棵二叉树的根节点 root ，按顺序返回组成二叉树 边界 的这些值。

示例 1：

![](https://github.com/Carmenliukang/ARTS/blob/master/image/32/2.jpg)

    输入：root = [1,null,2,3,4]
    输出：[1,3,4,2]
    解释：

    - 左边界为空，因为二叉树不含左子节点。
    - 右边界是 [2] 。从根节点的右子节点开始的路径为 2 -> 4 ，但 4 是叶节点，所以右边界只有 2 。
    - 叶节点从左到右是 [3,4] 。 按题目要求依序连接得到结果 [1] + [] + [3,4] + [2] = [1,3,4,2] 。 

示例 2：

![](https://github.com/Carmenliukang/ARTS/blob/master/image/32/3.jpg)

    输入：root = [1,2,3,4,5,6,null,null,null,7,8,9,10]
    输出：[1,2,4,7,8,9,10,6,3]
    解释：
    
    - 左边界为 [2] 。从根节点的左子节点开始的路径为 2 -> 4 ，但 4 是叶节点，所以左边界只有 2 。
    - 右边界是 [3,6] ，逆序为 [6,3] 。从根节点的右子节点开始的路径为 3 -> 6 -> 10 ，但 10 是叶节点。
    - 叶节点从左到右是 [4,7,8,9,10]
      按题目要求依序连接得到结果 [1] + [2] + [4,7,8,9,10] + [6,3] = [1,2,4,7,8,9,10,6,3] 。

提示：

    树中节点的数目在范围 [1, 104] 内
    -1000 <= Node.val <= 1000

### 分析

#### 分治算法

1. 左子树
2. 根节点
3. 右子树

#### 中序遍历方式

![](https://github.com/Carmenliukang/ARTS/blob/master/image/32/4.png)

从上图中可知，我们会发现问题描述很类似于先序遍历。实际上，遍历的顺序是一致的（除去右边界节点，它们是逆序的）。因此，我们只需要上述结果的顶点，属于左边界或者叶子节点或者右边界上。

为了区别这些节点，我们用 flagflag 符号：

- flag = 0：根节点
- flag = 1：左边界节点
- flag = 2：右边界节点
- flag = 3：其它（中间节点） 我们使用三个列表

left_boundary left_boundary，right_boundary right_boundary 和 leavesleaves 存储对应的节点。

我们按照正常的后序遍历的顺序，但当调用递归程序处理左孩子或者右孩子时，我们同时更新 flagflag 信息，表明这个节点孩子的类型。

当前节点的左孩子，我们使用函数 leftChildFlag(node, flag)。当处理左孩子时，下面可能的情况，可以通过上图来区分：

当前节点是左边界节点：左孩子也是左边界节点。例如，图中 E 和 J 的关系。 当前节点是根节点：左孩子也是左边界节点。例如，图中 A 和 B 的关系。 当前节点是右边界节点：如果右孩子不存在那么左孩子就是右边界节点。例如，上图中的 G 和
N。但是如果右孩子存在，左孩子只能作为中间节点，如图中 C 和 F。 相似地，也会有 rightChildFlag(node, flag) 来判断当前节点的右孩子：

当前节点是右边界节点：右孩子也是右边界节点。例如，图中 C 和 G 的关系。 当前节点是根节点：右孩子也是右边界节点。例如，图中 A 和 C 的关系。 当前节点是左边界节点：如果左孩子不存在那么右孩子就是右边界节点。例如，上图中的 B 和
E。但是如果右孩子存在，左孩子只能作为中间节点。 使用这些信息，我们更新 flagflag 然后用来决定那些节点会被加入到输出列表中。

### coding

```python


class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:
    def __init__(self):
        self.left_root = []
        self.right_root = []
        self.leaf_root = []

    def boundaryOfBinaryTree(self, root: TreeNode) -> list[int]:

        if root is None:
            return []

        if root.left:
            self.addleftnode(root.left)
        if root.left or root.right:
            self.addleafroot(root)
        if root.right:
            self.addrightnode(root.right)
        res = [root.val] + self.left_root + self.leaf_root + self.right_root[::-1]
        return res

    def isleaf(self, root):
        if root.left is None and root.right is None:
            return True
        else:
            return False

    def addleafroot(self, root):
        # 遍历叶子节点
        if self.isleaf(root):
            self.leaf_root.append(root.val)
        if root.left:
            self.addleafroot(root.left)
        if root.right:
            self.addleafroot(root.right)

    def addleftnode(self, root):
        # 左子树遍历，对于仅仅判断其最初的节点同步
        if root is None:
            return

        if not self.isleaf(root):
            self.left_root.append(root.val)

        if root.left:
            self.addleftnode(root.left)
        else:
            if root.right:
                self.addleftnode(root.right)

    def addrightnode(self, root):
        # 右子树遍历，对于仅仅判断其最初的节点同步
        if root is None:
            return

        if not self.isleaf(root):
            self.right_root.append(root.val)

        if root.right:
            self.addrightnode(root.right)
        else:
            if root.left:
                self.addrightnode(root.left)


```

***

## Review

> [The Secret Algorithm Behind Learning](https://medium.com/personal-growth/the-secret-algorithm-behind-learning-7c6f4eb702df)

### 概述

“The person who says he knows what he thinks but cannot express it usually does not know what he thinks.”

![](https://github.com/Carmenliukang/ARTS/blob/master/image/32/6.png)


#### the Feynman Technique
1. Step 1: Teach it to a child

2. Step 2: Review

3. Step 3: Organize and Simplify

4. Step 4 (optional): Transmit


***

## Tip

> [Redis-突然变慢归类](https://time.geekbang.org/column/article/286549)

### 概述

Redis 可能会突然变得很慢，展示具体的方法，然后依次进行判定。

#### 如何确定变慢

1. 机器性能方面：redis-cli 命令提供了–intrinsic-latency 查看 基线性能，如果运行时延迟时基准性能的2倍，那么就说明有延迟。

```shell
./redis-cli --intrinsic-latency 120
Max latency so far: 17 microseconds.
Max latency so far: 44 microseconds.
Max latency so far: 94 microseconds.
Max latency so far: 110 microseconds.
Max latency so far: 119 microseconds.

36481658 total runs (avg latency: 3.2893 microseconds / 3289.32 nanoseconds per run).
Worst run took 36x longer than the average latency.

```

2. 网络影响：iPerf 工具

#### 诊断过程

![](https://github.com/Carmenliukang/ARTS/blob/master/image/32/5.jpg)

1. 慢查询命令
    1. https://redis.io/commands/ 官网查询io时间
    2. Redis LOG
    3. latency monitor

2. 过期Keys 操作
    1. ACTIVE_EXPIRE_CYCLE_LOOKUPS_PER_LOOP 设置默认是 20
    2. 大量的 key 同一时间过期

***

## Share

> [关于习惯的掌控](https://zhuanlan.zhihu.com/p/355651730)

### 概述

1、睡前不刷抖音，每天至少多2小时自我提升时间。

2、不定期裸睡。

没有束缚的高质量睡眠，第二天醒来脑子会更清醒，不仅精力充沛，工作效率也会翻倍。

3、没事练练卷腹、臀桥。

尤其是学习久坐的人，练一练，能有效缓解腰酸背痛，提升学习效率的同时，还能拥有好看的马甲线/腹肌。

4、国际歌(唐朝版)设为闹铃，调到最大声放到伸手够不到的地方，再也不用担心赖床迟到。

5、巨提升社交效率的2个小习惯：

发微信，能打字讲明白的，绝不发语音。
凡是发「在吗」，后面不马上说事的，一律不回。
做到这两个小细节的人，生活的幸福感会直线上升。

6、多下片，下完后立刻将它们分类保存，特别是看一遍不过瘾的好资源！

如高质量TED、纪录片、豆瓣高分电影，分好类，想重刷时就能快速找到。

优质内容，每刷一次都会给你带来新的认知。

7、刷知乎看到好回答，双击屏幕，不仅能快速收藏，还能锻炼手指力量。

8、平时不在床上玩任何电子产品，只要带电的都不行！

9、愤怒时不做任何决定，愤怒下的决定，不仅容易伤害别人，自己消气后也多半会后悔。生气时，深呼吸三次，能有效缓解愤怒。

10、不论去哪，兜里随时揣一小包纸巾。

11、少吃甜食，每天八杯水，保持身材匀称只要管住嘴，就能少做很多又累又耗时的燃脂运动。

12、坚持每天中午眯15-20分钟，就能换下午4小时高效工作状态。

13、刚播的新剧，出完再追，能极好地锻炼自己延迟满足的能力。

14、用早起带动早睡。坚持3天7点起床，基本就能保证每天11点入睡。

然后你会发现慢悠悠享受完早餐，再去上班，比随便买份早餐，再赶到公司狼吞虎咽要爽100倍。

15、接上一条，如何让自己快速无痛苦早起？

晚上窗帘留一条缝，窗户开一点，远比定10个闹钟更有效。

因为房间二氧化碳浓度不高，同时阳光照射，能让你第二天更舒服、快速地自然醒。

16、无聊时可以上网搜集各种资源进行自学。大学里我就靠自学，赚到了人生中的第一个10万，即使现在毕业了，我仍没有懈怠，保持着这个习惯。

现在互联网时代，多学一门技能，就意味着你多一条出路，即使现在你有一份稳定的工作，把它们发展为副业，也能轻松带来不错的额外收入。我曾经花了几万块钱买了很多关于Excel、PPT、PS、手绘等教程和素材，现已全部打包好了，想让人生拥有更多可能性的同学，可以关注我的公众号【铁木君】拿到这份分享，大大节省你搜集的时间，让你迅速掌握新技能，轻松吊打同龄人~

17、每天晚上抽5分钟做两件事，能让第二天效率提升200%。

一、简单写一下明天要做的事；二、把要穿的衣服叠好放床头。
18、用1.5倍速刷剧、学习视频，不仅能节省大量时间，还可以锻炼专注力。

19、没事给知乎收藏夹扫扫灰。

有时候不是方法太少、改变太难，可能只是因为点完「★收藏」，就把它们丢到了一边。

20、深夜的酒局不参加，能让你有效避开睡眠不足、身材走形的烦恼。

21、工作、学习累了，抽5分钟数一下呼吸。

呼，数1；吸，数2；如此循环。
给大脑做一个短暂但深度的冥想spa，能快速恢复你的精力、专注力。

22、压力很大，觉得丧的时候，出门到菜场、广场逛一逛。听听大妈们的唠嗑，小贩的招揽声，心情也会变得平稳。

23、生活中多赞赏别人。

谈恋爱，嘴甜更受女生欢迎。

工作上，和同事相处会更融洽。

如果不会说好听的话，可以用一些温暖的小动作替代，比如认同别人，给答主点赞，也会给自己带来好运~

24、学会控制自己的热心肠。

别人遇到问题，如果没有主动请教你，不要随便给建议，因为好心的建议很可能被当成「炫耀」。

25、如果戒不掉抖音，就反向利用它。

比如把手机放在跑步机上，边跑边刷。

26、交朋友时，不要只看自己能得到什么，有时候为别人考虑一下，自己先付出一点，远能比只顾自己获得更多。

27、少看游戏、购物直播，钱和时间都会越来越缺。

28、有事没事笑一笑，不仅能提升抗压能力，还能让你更受欢迎。

29、果断关掉花呗、白条。

不要觉得几千几万额度花着很爽，现在你有多爽，还款时你就有多心痛。

30、从最简单的记账开始,逐步学会理财。

将收入分为不动产和正常花销，每天记录钱花在哪里，比如三餐、交通费、网购、娱乐等，坚持记账，你就能由此判断自己的消费结构，并作出合理的调整，至少能规避90%的经济危机。

对大多人来说，理财是实现经济独立最有效的方式，即使收入不高，也能实现资金的最大化收益。如果你想学理财又不知从何下手，可以关注我的公众号【铁木君】，我把花了几万向金融大佬求教到的理财经验分享给你，吸收内化后，能让你迅速打破经济壁垒，早日实现经济独立~

31、这些小细节，社交中能迅速增加你的亲和力。

新认识一个朋友，问清对方名字，并念一次；记住对方来自哪里，和他聊一些他家乡的事；觉得对方的一些观点有道理时，毫不吝啬地给他点赞。

32、再忙，每天也至少读10页书。

每天不到10分钟，但一年下来，最少能读完10本书。

33、高质量的书至少看两遍。

第一遍尽情享受，这样第二遍再思考、内化时会更深刻、专注。

34、让环境干净一点，比疯狂买护肤品更有效。

比如回家立马换下身上的脏衣服，换上居家服再上床；贴身衣物和袜子分开洗。

35、利用仪式感快速切换状态。

比如学习前洗个手，工作前把桌子擦一下，养成习惯后，这些小细节能让你更快、更好地切换到另一个状态。

36、坚持每天5-10分钟平板支撑，轻松拥有腹肌马甲线。

37、早起喝一杯温水，不仅能快速唤醒大脑，还能补充体内水分，促进新陈代谢，让你的皮肤更细腻光滑。

38、心烦意乱时，进行深呼吸。

深吸，吸到不能再吸；憋4秒；再长长吐出，吐到一点都吐不出来。

循环2-5次，能快速清除干扰，让你重回专注，开启深度思考模式。

39、不强迫自己同三观不和的人交朋友。

低质量的合群不如高质量的独处。
40、最后，看完回答并点赞的人，后续会收到更多优质推送，会比大多数人获得更多一手知识。

