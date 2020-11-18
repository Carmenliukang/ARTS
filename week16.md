# ARTS Week 
>[todo](todo)
>> todo

***
## Algoithm
>[todo](todo)

todo

***
## Review
>[When DRY Doesn’t Work, Go WET](https://medium.com/better-programming/when-dry-doesnt-work-go-wet-6befda0444bf)

### 概述
这里主要是使用了相关的状态同步


***
## Tip
>[todo]()

todo


***
## Share
>[N叉树构建](https://github.com/Carmenliukang/ARTS/blob/master/week16.md#share)


### 概述
```python

class Node(object):
    def __init__(self, val=None):
        self.val = val
        self.children = []  # 这里必须是一个有序，所以选择的是数组


class Tree(object):
    # 创建N叉树
    def create(self, val, data):
        # 递归调用创建N叉树
        root = Node(val=val)
        for i in data.get(val, []):
            node = self.create(val=i, data=data)
            root.children.append(node)
        return root

    def show(self, root):
        for i in root.children:
            self.show(i)


if __name__ == '__main__':
    node = []
    data = {
        1: [2, 3],
        2: [6, 7],
        7: [5]
    }

    tree = Tree()
    root = tree.create(1, data)

    tree.show(root)

```  