---
layout: post
title: "ActiveJDBC笔记：3 编程技巧"
categories: Java
tags: Java Java框架
excerpt: ActiveJDBC相关使用技巧。
author: "Eric Zong"
---

* content
{:toc}

# 编程技巧

ActiveJDBC 提供了很多便捷方法，使得编码更为简洁。

## 链式编程

model.set() 方法返回当前模型对象，所以可以进行链式编程：

```java
new Person().set("first_name", "Marilyn").set("last_name", "Monroe").set("dob", "1935-12-06").saveIt();
```

## 批量赋值

model.set() 方法有一批量赋值的重载形式 set(String[] attributeNames, Object[] values)。

## Map 初始化

model.fromMap(map) 可以使用 Map 为模型对象赋值。

## 命令行参数式初始化

model.set(Object... namesAndValues) 可以类似命令行参数的形式给模型对象赋值。

## 命令行参数式创建

Model.create(Object... namesAndValues) 和 Model.createIt(Object... namesAndValues) 也提供上面类似的方式创建模型对象。

## 检查有效属性

当 model.set(name, value) 设置的字段不存在时，将抛出运行时异常。

不过，通常可以故意导致这个异常，用以查看异常信息中列出的可用属性。

