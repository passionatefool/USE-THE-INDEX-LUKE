##  DELETE

不同于 `insert` 语句， `delete` 语句有一个 `where` 子句，它可以使用[第二章 “Where子句”](./where-clause.md)中描述的所有方法，并直接从索引中受益。事实上，`delete` 语句和 `select` 一样，不过 `delete` 会在 `select` 后面加一个额外的步骤来删除已识别的行。

行的实际删除与插入新行的过程类似——尤其是从索引中删除引用以及保持索引树平衡的过程。因此，[图 8.2 ](#图8.2 Delete性能和索引数量关系) 中显示的性能图表与[插入](./8-1-insert.md)的性能图表非常相似。



#### 图8.2 Delete性能和索引数量关系

<img src="./img/fig08_02_delete.en.Spgqwe-H.png" alt="img" style="zoom:150%;" />



从理论上讲，对于没有任何索引的表，我们期望得到最佳的删除性能——就像插入章节提到的一样。但是，如果没有索引，则数据库必须扫描全表才能找到要删除的行。这意味着删除行会很快，但是查找会非常慢。因此，[图8.2](#图8.2 Delete性能和索引数量关系)中未显示这种情况。

尽管如此，在没有索引的情况下执行  `delete` 语句是有意义的，就像在没有索引的情况下执行 `select` 语句（如果它返回表中一大部分数据）是有意义的一样。



> 💡TIP
>
> 删除和更新语句也有执行计划。



没有 `where` 子句的 `delete` 语句是数据库不能使用索引的一个明显例子，尽管这是一个特殊情况，它有自己的 SQL 命令：`truncate table`。此命令与不带 `where` 子句的 `delete` 效果相同，只是它一次性删除所有行。它非常快，但有两个重要的副作用：(1) 它执行隐式提交（例外：PostgreSQL 和 SQL Server）； (2) 它不执行任何触发器。



>MVCC的副作用
>
>多版本并发控制(MVCC)是一种数据库机制，它支持非阻塞并发数据访问和一致的事务视图。然而，实现因数据库而异，甚至可能对性能产生相当大的影响。
>
>例如，PostgreSQL数据库只在表级别上保留版本信息(=可见性信息)：删除一行只是在表块中设置了删除标志。因此，PostgreSQL的删除性能不依赖于表上索引的数量。表行的物理删除和相关的索引维护只在[VACUUM](https://www.postgresql.org/docs/current/static/sql-vacuum.html)过程中执行。
