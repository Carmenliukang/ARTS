# ARTS Week 3
***
![图片](https://s1.ax1x.com/2020/06/03/tUc3LD.jpg)
> **什么也无法舍弃的人，什么也改变不了。  --《进击的巨人》by 阿尔敏**
***

## Algoithm
> [拥有最多糖果的孩子](https://leetcode-cn.com/problems/kids-with-the-greatest-number-of-candies/)

### 概述
    这是一个非常简单的题目
    给你一个数组 candies 和一个整数 extraCandies ，其中 candies[i] 代表第 i 个孩子拥有的糖果数目。

    对每一个孩子，检查是否存在一种方案，将额外的 extraCandies 个糖果分配给孩子们之后，此孩子有 最多 的糖果。注意，允许有多个孩子同时拥有 最多 的糖果数目。
    
     
    
    示例 1：
    
    输入：candies = [2,3,5,1,3], extraCandies = 3
    输出：[true,true,true,false,true] 
    解释：
    孩子 1 有 2 个糖果，如果他得到所有额外的糖果（3个），那么他总共有 5 个糖果，他将成为拥有最多糖果的孩子。
    孩子 2 有 3 个糖果，如果他得到至少 2 个额外糖果，那么他将成为拥有最多糖果的孩子。
    孩子 3 有 5 个糖果，他已经是拥有最多糖果的孩子。
    孩子 4 有 1 个糖果，即使他得到所有额外的糖果，他也只有 4 个糖果，无法成为拥有糖果最多的孩子。
    孩子 5 有 3 个糖果，如果他得到至少 2 个额外糖果，那么他将成为拥有最多糖果的孩子。
    示例 2：
    
    输入：candies = [4,2,1,1,2], extraCandies = 1
    输出：[true,false,false,false,false] 
    解释：只有 1 个额外糖果，所以不管额外糖果给谁，只有孩子 1 可以成为拥有糖果最多的孩子。
    示例 3：
    
    输入：candies = [12,1,12], extraCandies = 10
    输出：[true,false,true]
     
    
    提示：
    
    2 <= candies.length <= 100
    1 <= candies[i] <= 100
    1 <= extraCandies <= 50
    

### 思考
1. 计算最大数值
2. 遍历 增加 糖果数量 依次对比
3. 返回最后结果 list

### 代码
```python
class Solution:
    def kidsWithCandies(self, candies: list[int], extraCandies: int) -> list[bool]:
        res=  []
        candies_max = max(candies)
        for i in candies:
            res.append(True if i+extraCandies >= candies_max else False)
        return res
```

***
## Review
> [If You Want to Be a Senior Developer, Stop Focusing on Syntax](https://medium.com/better-programming/if-you-want-to-be-a-senior-developer-stop-focusing-on-syntax-d77b081cb10b)

### 概述
这片文章，主要借用，通过地图，也可以在陌生城市找到目的地，同时熟悉以后，可以摆脱地图达到目的地。
类比现在的工程师行业，每天都在不断的变化和更新，一个人是不可能学会所有的语法，会使用 python golang java net lua js 等。
这些对于你以后的发展会有哪些好处？所以如果可以更加需要的是深入理解底层核心部分，不论哪一种，底层核心的变化其实都不是很大。

### 思考
文章说的确实是有一定的道理，同时也要相对的扎根当前和不久的将来，同时也要参考未来，进行一个相对平衡的点的确定，这样的才会尽量在以后不会后悔。
同时也保证了方向上的正确性。
* 初级开发师 确实需要学习 一些基础的 语法，因为需要工作和生活，这部分需要的就是快速学习，尽量对比的学习，同时能够做到类比，比较两者之间的不同，以及为什么会不同，这样的一个深度。
* 中级工程师 就要对底层有一定的了解，因为万事万物 都是 殊途同归，所以需要对于底层有一定的了解，比如：不同的语言，他的 gc/内存分配 算法调度 等等都是需要需要思考。尽可能的脱离语法/语言 去思考。
* 高级工程师 就是已经对于底层有了深入了解，能够建立一套完整的体系，包含了从 CPU运行/调度 内存 磁盘IO 等等，做到 庖丁解牛的地步。
* 架构师级别 是能够从 业务 角度 去看待技术，采用的架构，需要平衡 现在/近期/未来。


***
## Tip
> [索引 哈希索引/BST/AVL/红黑树/B树/B+树 优缺点](https://zhuanlan.zhihu.com/p/84493668)

### 概述
1. 哈希索引：
    * 通过算法，计算获得其磁盘位置，然后直接读取IO，获取数据。
    * 对于 等值查询 是非常不错的方式
    * 不支持范围查询，同时如果哈希结果一样，等同于遍历方式。

2. BTS 索引：
    * 任意节点的左子树上所有节点值不大于根节点的值，任意节点的右子树上所有节点值不小于根节点的值。如下是一颗BST
    ![实例图片](https://pic1.zhimg.com/v2-bd67453dc3a932b4d614ca1b0735cd2c_r.jpg)
    * 能够快速找到数据
    * 数据过大，导致层过高。

3. AVL 索引：
    * 节点的左右子树高度差不能超过1
    ![实例图片](https://pic2.zhimg.com/80/v2-059e1c5599668a01e17bd5561cdf8865_720w.jpg)
    * 查找性能稳定
    * 需要维持 平衡，删除和增加都会有很大的性能损耗，因为需要保持 从 根节点到子节点 的平衡。
 
4. 红黑树:
    * 追求大致的平衡，
    ![](https://pic4.zhimg.com/80/v2-5e526cc61f4e124c60f39721285b1d5b_720w.jpg)
    * 在内存中查找有很大的提升，同时在 删除/查找/更新 中最有 O(1) 的时间。
    * 层数还是非常的高，磁盘IO还是太多了。

5. B树
    * 多路平衡查找树,一个非 根节点，可以有多个子节点。
    ![](https://pic2.zhimg.com/80/v2-eb70015708101159e6a1c37c9afc0dc1_720w.jpg)
        * 每个节点最多包含 m 个子节点。
        * 如果根节点包含子节点，则至少包含 2 个子节点；除根节点外，每个非叶节点至少包含 m/2 个子节点。
        * 拥有 k 个子节点的非叶节点将包含 k - 1 条记录。
        * 所有叶节点都在同一层中。
    * 大大降低了IO索引，提高性能。
    * 节点保存了数据，所以数据量会很大。

6. B+树
    * 区别 
        * B树中每个节点（包括叶节点和非叶节点）都存储真实的数据，B+树中只有叶子节点存储真实的数据，非叶节点只存储键。在MySQL中，这里所说的真实数据，可能是行的全部数据（如Innodb的聚簇索引），也可能只是行的主键（如Innodb的辅助索引），或者是行所在的地址（如MyIsam的非聚簇索引）。
        * B树中一条记录只会出现一次，不会重复出现，而B+树的键则可能重复重现——一定会在叶节点出现，也可能在非叶节点重复出现。
        * B+树的叶节点之间通过双向链表链接。
        * B树中的非叶节点，记录数比子节点个数少1；而B+树中记录数与子节点个数相同。
       
   
    
### 思考
为什么选择B+树？而MongoDB 使用B树，其实还是因为存储的数据结构不同的问题，对于关系型数据库，需要存储的数据是有一定的格式规范的，但是如果是
非关系型数据，那么更多的是依赖数据的可变性，所以如果使用B+树，那么如果修改数据，就需要进行全量的数据更新，因为需要维持一个统一的数据规范格式，
但是 非关系型 数据 却不一样，他需要的是保证的是 数据存储的 便捷性，能够更加方便的提高数据的可视化性能，所以使用了B树。

那么再次深入一下，一切的使用都是依据不同的业务场景，进行匹配。所以需要深入了解业务后，确定最后的具体方案，然后才确定最后需要的技术。

***
## Share
> [mysql 幻读](https://time.geekbang.org/column/article/75173)

### 概述
* 主要讲述了 mysql 幻读/脏读 出现的一些基本情况。
* 读未提交/读已提交/可重复读/串行读取
* 间隙锁 这种方式可以 规避 幻读，但是会降低系统的并发性能。


### 思考
1. 是否可以通过第三方的 redis 等进行热点数据的同步。
2. 在业务方面 是否 需要 规避幻读？
    
