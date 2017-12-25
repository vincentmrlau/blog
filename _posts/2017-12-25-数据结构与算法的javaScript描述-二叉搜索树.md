---
title:  数据结构与算法对的javaScript描述-二叉搜索树
date:   2017-12-25 17:00:18
tags:
- 二叉搜索树
- 数据结构
- 算法

excerpt: 二叉搜索树（Binary Search Tree）是计算机科学中非常常用的一种数据结构。在BST上添加、查找、删除等对数据的操作都非常的快，本文借助 https://visualgo.net/zh/bst 用js来描述二叉搜索树。
published: true
---

# 定义
## 树以及相关概念

![](https://github.com/vincentmrlau/remote-image-store/raw/master/tree-definations.png)

## 二叉搜索树的定义
* 首先是二叉树
	* 最多有两个节点，分别是左右节点
* 子节点的位置是确定的，变现为
	* 左子节点的值小于其父节点
	* 右子节点的值大于其父节点

# BST在JS中的描述
> [JS描述的完整代码传送门](https://github.com/vincentmrlau/classic-subject-web-fe/blob/master/js/data-structures-and-algorithms/BST.js)
> [可视化BST传送门](https://visualgo.net/zh/bst)

## 节点类 Node
> 树是由节点组成的，要实现树那么先要实现节点

### 节点的要素
* data:每个节点都需要有一个数值
* left:左子节点，在没有左子节点的时候指向空对象null
* right: 右子节点，在没有右子节点的时候指向空对象null
* count: 计数值，累计插入的次数，这个是非必须的，作为一个要素是为了后续处理插入重复值的问题
* show: 显示方法，展示该节点的值（和插入次数），在这里返回一个包含值（和插入次数）的对象

### js的描述
```javaScript
class Node {
  constructor (data,
               count = 1,
               left = null,
               right = null)
  {
    // 数值
    this.data = data
    // 出现次数
    this.count = count
    // 左右节点指向
    this.left = left
    this.right = right
  }
  show () {
    return {
      data: this.data,
      count: this.count
    }
  }
}

```

## BST 二叉搜索树类
### 属性
目前我们只需要一个根节点的属性，因此一个基本的`BST`可以描述为:

```javaScript
class BST {
  constructor () {
    // 初始化跟节点为null
    this.root = null
  }
}
```

### 插入方法
我们有了一个基本的`BST`,这时候我们可以`new`一个 `bst`，那么我们怎么插入数据呢？这时候我们需要一个`insert`方法，这个方法有以下的条件：

* 插入的是节点，也就是上述的类`Node`的一个实例
* 如果没有根节点，`bst`的根节点指向该节点
* 如果有根节点则向下遍历，找到合适的位置插入该节点，遍历规则如下图：

![](https://github.com/vincentmrlau/remote-image-store/raw/master/bst-insert.png)

#### 带有插入方法的BSTjs的描述如下

```javaScript
class BST {
  constructor () {
    // 初始化跟节点为null
    this.root = null
  }
  /**
   * 插入数据
   * @param data
   */
  insert (data) {
    let n = new Node(data, 1)
    if (this.root === null) {
      // 没有根节点，新的树把待插入的值作为根节点
      this.root = n
    } else {
      // 有根节点，遍历树直到找到合适的位置
      let current = this.root
      while (true) {
        if (data < current.data) {
          if (current.left === null) {
            current.left = n
            break
          }
          current = current.left
        } else if (data === current.data) {
          current.count += 1
          break
        } else {
          if (current.right === null) {
            current.right = n
            break
          }
          current = current.right
        }
      }
    }
  }
}
```
#### 插入一组测试数据测试

```javaScript
let testData = [
  43,
  34,
  67,
  23,
  34,
  45,
  2,
  78,
  34
]

let bst = new BST()
console.log(JSON.stringify(bst))

for (let data of testData) {
  bst.insert(data)
}
console.log(JSON.stringify(bst))
```
插入数据前：

```JSON
{"root":null}
```
插入数据后

![](https://github.com/vincentmrlau/remote-image-store/raw/master/bst-testdata.png)

```JSON
{
    "root": {
        "data": 43,
        "count": 1,
        "left": {
            "data": 34,
            "count": 3,
            "left": {
                "data": 23,
                "count": 1,
                "left": {
                    "data": 2,
                    "count": 1,
                    "left": null,
                    "right": null
                },
                "right": {
                    "data": 28,
                    "count": 1,
                    "left": null,
                    "right": null
                }
            },
            "right": null
        },
        "right": {
            "data": 67,
            "count": 1,
            "left": {
                "data": 45,
                "count": 1,
                "left": null,
                "right": null
            },
            "right": {
                "data": 78,
                "count": 1,
                "left": null,
                "right": null
            }
        }
    }
}
```
插入数据之后我们是通过nodejs的logger来查看`bst`，事实上，我们还需要其他的遍历方法来查看`bst`

### BST的遍历
> 遍历二叉树通常有三种遍历方法，分别是中序、先序和后序，他们的遍历路径不一样

#### 中序遍历
> 中序遍历应该是最常用的一种遍历方法

js中的描述

```javaScript
/**
   * 中序遍历
   * @param node
   */
  inOrder (node) {
    if (node !== null) {
      this.inOrder(node.left)
      console.log(`data:${node.data},count:${node.count}`)
      this.inOrder(node.right)
    }
  }
```
上述例子中的输出结果

```bash
中序
data:2,count:1
data:23,count:1
data:28,count:1
data:34,count:3
data:43,count:1
data:45,count:1
data:67,count:1
data:78,count:1
```
路径图
![](https://github.com/vincentmrlau/remote-image-store/raw/master/bst-inorder.png)

#### 先序遍历
js中的描述

```javaScript
  /**
   * 先序遍历
   * @param node
   */
  preOrder (node) {
    if (node !== null) {
      console.log(`data:${node.data},count:${node.count}`)
      this.preOrder(node.left)
      this.preOrder(node.right)
    }
  }
```
上述例子中的输出结果

```bash
先序
data:43,count:1
data:34,count:3
data:23,count:1
data:2,count:1
data:28,count:1
data:67,count:1
data:45,count:1
data:78,count:1
```
路径图
![](https://github.com/vincentmrlau/remote-image-store/raw/master/bst-preorder.png)

#### 后序遍历
js中的描述

```javaScript
  /**
   * 后序遍历
   * @param node
   */
  postOrder (node) {
    if (node !== null) {
      this.postOrder(node.left)
      this.postOrder(node.right)
      console.log(`data:${node.data},count:${node.count}`)
    }
  }
```
上述例子中的输出结果

```bash
后序
data:2,count:1
data:28,count:1
data:23,count:1
data:34,count:3
data:45,count:1
data:78,count:1
data:67,count:1
data:43,count:1
```
路径图
![](https://github.com/vincentmrlau/remote-image-store/raw/master/bst-postorder.png)

### 查找
> 在二叉搜索树中查找数据非常简单

#### 最大值与最小值
> 最小值为树中的最左边的叶子节点得到值，最大值为最右边子节点的值

```javaScript
  /**
   * 查找最小值
   * @returns {CanvasPixelArray|string|Object[]|*}
   */
  getMin (node) {
    let current = node || this.root
    while (current.left !== null) {
      current = current.left
    }
    return current
  }
  
  /**
   * 查找最大值
   * @returns {CanvasPixelArray|string|Object[]|*}
   */
  getMax (node) {
    let current = node || this.root
    while (current.right !== null) {
      current = current.right
    }
    return current
  }
```
#### 查找指定数据
> 根据数据的大小判断向左向右查找使得查找非常有效率

```javaScript
  /**
   * 查找数据
   * @param data
   * @returns {*}
   */
  find (data) {
    let current = this.root,
      result = null
    while (current !== null) {
      if (data === current.data) {
        result = current
        break
      } else if (data < current.data) {
        current = current.left
      } else {
        current = current.right
      }
    }
    return result
  }
```

```bash
bst.getMax().data // 78
bst.getMax().count // 1
bst.getMin().data // 2
bst.getMin().count // 1
```

### 删除数据
> 删除数据其实是操作二叉搜索树中最麻烦的一部分

我的思路如下：

* 待删除节点没有子节点：
	* 父节点链向该节点的链接指向null
* 待删除节点只有左节点或者右节点
	* 父节点链向他的链接指向他的子节点
* 待删除节点既有左节点又有右节点
	* 用他的右子树的最小节点取代（值和计数替代）待删除节点
	* 在他的右子树中删除右子树中的最小节点

基于此，JS中的描述为

```javaScript
  /**
   * 删除数据
   * @param data
   */
  remove ( data ) {
    this.root = this.removeDataFromNode(this.root, data)
  }
  
  /**
   * 从指定节点中删除数据
   * @param node
   * @param data
   * @returns {*}
   */
  removeDataFromNode (node, data) {
    if (node !== null) {
      if (data === node.data) {
        if (node.left === null && node.right === null) {
        //  没有子节点
          return null
        }
        if (node.right === null) {
        //  只有左节点
          return node.left
        }
        if (node.left === null) {
        //  只有右节点
          return node.right
        }
        // 有做节点和右节点
        // 取右节点的最小值
        let tempNode = this.getMin(node.right)
        node.data = tempNode.data
        node.count = tempNode.count
        node.right = this.removeDataFromNode(node.right, tempNode.data)
        return node
      } else if (data < node.data) {
        node.left = this.removeDataFromNode(node.left, data)
        return node
      } else {
        node.right = this.removeDataFromNode(node.right, data)
        return node
      }
    } else {
      return null
    }
  }
```

