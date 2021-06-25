# 解剖一条索引

我见过的大多数对索引的基本解释都是索引能使查询变快。虽然这个解释已经很好地描述了索引最重要的方面，
但是对于这本书来说显然是不够的。本章会以一种不那么浅显的方式描述索引的结构，但也不会太深入细节。
它可以给你提供足够的视角来理解全书中讨论的 SQL 性能方面的知识。

索引是数据库中使用 `CREATE INDEX` 语句来构建的一种特殊结构。它需要有自己的磁盘空间并持有一份索引数据的副本。
这意味着索引是冗余的。创建索引不会改变表的数据，它只是创建了一个引用该表的新的数据结构。
总的来说，数据库索引就像本书的索引目录一样：它有自己的空间，它是高度冗余的，它指向的是存储在不同地方的原始数据。

数据库索引的检索其实就像在电话簿目录中检索一样。这里面的关键是所有的条目都按照定义好的顺序排列。
在有序的数据集中查找数据是非常快且容易的，因为排序的顺序决定了每个条目的位置。

但是，数据库索引比电话簿目录更加复杂，因为它要经历不断的变化。为每个更改去更新电话簿目录是不可能的，
原因很简单，即现有的条目之间可能已经没有空间来添加新的条目。电话簿目录只能在下一次重新打印的时候来处理
累计下来的更新从而来绕过这个问题。但是，SQL 数据库可等不了这么长时间，它必须立即处理增删改语句，
要做到保持索引顺序不变的情况下且不移动大量的数据。

数据库结合了两种数据结构来应对该挑战：双向链表和搜索树。这两个数据结构可以解释数据库的大部分性能特征。

## 内容

1. *[叶子节点](./the-leaf-nodes.md)* -- 双向链表
2. *[B-Tree](./the-tree.md)* -- 平衡树
3. *[慢索引，第一部分](./slow-indexes.md)* -- 两个因素使索引变得缓慢