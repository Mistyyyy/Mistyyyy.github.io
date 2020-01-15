---
title: 链表的操作与技巧的运用
date: "2020-01-15T21:00:00.284Z"
description: "链表 快慢指针"
---

链表是一种很常见的，线性的数据结构。由一个个节点构成。每一个节点都是一个对象，
包含当前的值与指向下一个节点的指针。数组用索引将不同的元素连接起来，链表用指针将前后的元素连接起来。

```javascript

function Node(val, next = null) {
  this.val = val;
  this.next = next;
}

```

> 链表可以划分为单链表，循环单链表，双链表，循环双链表。下面使用
> 单链表作为实例来讲解。
> 以下动画全是来源于 [visualgo](https://visualgo.net/zh/list)

### 链表数据结构的特殊性

链表是一种线性的数据结构，由多个节点组成，每一个节点指向下一个节点。具有天然的递归性质。此外，一个链表也可以看作是
一个完全不平衡的二叉树。可以利用其递归性来解决很多问题。

如: 合并两个有序链表，利用其递归性质，可以这样写:

```javascript

var mergeTwoLists = function(l1, l2) {
  if (l1 === null) return l2;
  if (l2 === null) return l1;
  if (l1.val < l2.val) {
    l1.next = mergeTwoLists(l1.next, l2);
    return l1;
  } else {
    l2.next = mergeTwoLists(l1, l2.next);
    return l2;
  }
}

```

链表与数组的操作对比，都具备可遍历，查找，修改，删除操作。由于节点与节点之间是通过指针来进行连接。插入，修改操作在已知节点的情况下，时间复杂度可以达到O(1)。而数组在在同等的情况下，需要考虑后续元素的位置移动。而在查找的情况下，数组可以通过索引快速的找到目标元素。而链表只能通过遍历来找到目标元素。两种数据结构在不同的使用场景下，操作效率也会大不相同。

下面，通过一些示例代码与动画进行链表操作的演示。

#### 查找

在链表中查找元素，需要不断的遍历链表，将每一个节点的值与目标匹配。如果匹配，那么返回该节点。否则，指针指向下一个节点。

```javascript

function find(node, target) {
  // 设置临时变量，不改变原数据
  let origin = node;
  while(origin) {
    if (origin.val === target) return origin;
    origin = origin.next;
  }
  return null;
}
```
在链表中查询值为 76 的节点

![在链表中查询值为76节点](./assets/node1.gif)

#### 插入

在链表中插入元素，如果在已知节点的情况下，直接改变相关节点的指针指向即可。如果在未知节点的情况下。需要先查找相关节点。再改变
相关节点的指针指向。

该过程可以分为三个步骤

1. 构建需要插入的节点，称作为新节点
2. 新节点的 next 指向目标节点的 next
3. 目标节点的 next 指向新节点

```javascript

const current = find(target);
// 1. create new node
const newNode = new Node();
// 2. 新节点的next指向目标节点的next
newNode.next = current.next;
// 3. 目标节点的next指向新节点
current.next = newNode;
```

在值为 43 的节点后插入值为 22 的节点

![插入](./assets/node_insert.gif)

#### 删除

在链表中删除一个节点，如果在已知节点的情况下，直接操作相关节点的指针即可。如果在未知节点的情况下，可以先查找相关节点，再改变
相关节点的指针指向。

**注: 这里的已知节点是待删除节点的前一个节点**

该过程可以分为两个步骤

1. 找到待删除节点的前一个节点(称为已知节点)
2. 将待删除的节点的 next 节点赋值给临时节点
3. 改变已知节点的next指向临时节点并将待删除的节点的 next 指向 null

```javascript
// 已知节点, 可能为 null
const preNode;
// 待删除节点, 假设存在
const deletes;

const temp = deletes.next;

if (preNode) preNode.next = temp;
// 否则待删除节点是头节点，重新定义链表的头节点
else head = deletes.next;

deletes.next = null;
```

删除值为43的节点

![删除](./assets/node_delete.gif)

### 链表的常见技巧的应用

链表作为一种线性，递归型，具有指针指向的数据结构。操作链表的同时即是在操作指针。在基本算法的技巧中，双指针，三指针这种技巧
时常被使用到，而链表天然的结构特性，注定着双指针可以解决链表中的很多特性。

下面，列举一些常见的问题，然后利用双指针来解决这些问题。

1. 如何找链表的中间节点。
2. 如何判断链表中是否存在环
3. 如果链表有环，如何确定环的入口
4. 判断相交链表的交叉节点
5. 如何旋转链表

> 以下题目与图片均来自于 [leetcode](https://leetcode-cn.com/)

#### 找链表的中间节点

约束: 给定一个带有头结点 head 的非空单链表，返回链表的中间结点。如果有两个中间结点，则返回第二个中间结点。

```javascript

输入: 1 -> 2 -> 3 -> 4 -> 5

输出: 3

输入: 1 -> 2 -> 3 -> 4 -> 5 -> 6

输出: 4

```





