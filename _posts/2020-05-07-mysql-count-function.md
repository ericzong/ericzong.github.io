---
layout: post
title: MySQL笔记：count函数
category: 数据库
tags: MySQL
excerpt: 介绍MySQL中count函数的几种用法以及差异。
author: Eric Zong
---

* content
{:toc}
# count 函数的作用及用法

MySQL 中，`count()` 函数的作用就是字面意思——计数。

`count(expr)` 计算的是指定的表达式的值不为 `NULL` 的数量。

> 对于 `count(*)` 而言，它统计的是行数，即使所有字段都为 `NULL`。可以理解为，统计行数据，而一行存在就不会是 `NULL`。

通常，它有三种用法：`count(*)`、`count(常量)`、`count(字段名)`。

# 三种用法的区别

首先，`count(*)` 与 `count(常量)` 结果是一致的，都是统计的行数。这种场景下，我们应该总是使用 `count(*)`，因为它是 SQL92 定义的标准统计行数的语法——跨数据库积噢！

另一方面， `count(字段名)` 统计的是该字段值不为 `NULL`  的数量。

# 性能对比

据 MySQL 官方文档所说，在 InnoDB 中，`count(*)` 和 `count(常量)` 是以相同方式执行的，没有性能差异。

相对而言，`count(字段名)` 需要进一步对字段判空，所以性能是最差的。

> **关于 `count(*)` 的优化**
>
> MySQL 为 `count(*)` 进行全表行数查询做了一些优化。
>
> 在 MyISAM 中，不支持事务，串行操作无并发，所以会单独记录表行数，以提升查询全表行数的性能。
>
> 在 InnoDB 中，支持事务及行级锁，存在并发操作，不能使用“记录表行数”的方式优化，而是使用非聚簇索引来扫表。
>
> 注：InnoDB 中索引分为聚簇索引（主键索引）和非聚簇索引（非主键索引），聚簇索引的叶子节点中保存的是整行记录，而非聚簇索引的叶子节点中保存的是该行记录的主键的值。所以，相比之下，非聚簇索引要比聚簇索引小很多，所以 MySQL 会优先选择最小的非聚簇索引来扫表。可见，创建一个非主键索引的必要性。

# 参考

https://dev.mysql.com/doc/refman/8.0/en/group-by-functions.html#function_count