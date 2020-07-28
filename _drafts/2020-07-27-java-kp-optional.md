---
layout: post
title: ”Java知识点：Optional类的使用"
category: Java
tags: Java Java基础 Java知识点
excerpt: Java类Optional的使用说明。
author: Eric Zong
---

* content
{:toc}

# 简介

`Optional` 是 Java 8 新增的一个工具类。它是一个容器，用以包装一个值对象。

使用 `Optional` 可以大大消除充斥于代码中的“判空“操作，使代码更加优。

# 创建实例

如果查看 `Optional` 的源码可以发现，它只有一个私有的构造器。因此，不能通过构造器创建其对象，
只能通过 3 个静态工厂方法来创建。

```java
Optional<T> empty()
Optional<T> of(T value)
Optional<T> ofNullable(T value)
```

`empty()` 用于创建一个“空”的 `Optional` 对象，它不包含任何值。
值得注意的是，`empty()` 返回的是 `Optional` 内部缓存的同一个对象，我们暂称之为“缓存空对象”。
更进一步，`Optional` 方法所返回的“空”对象通常也都是这个“缓存空对象“。

`of()` 用于创建一个“非空”的 `Optional` 对象，因此，如果传入 `null` 将会抛出“空指针”。

`ofNullable()` 顾名思义，它可以创建一个任意的 `Optional` 对象，即使它是“空”的。但是，如果传入 `null` 参数则还是会返回“缓存空对象”，而不会另创建一个“空”对象。

