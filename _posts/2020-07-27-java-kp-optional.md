---
layout: post
title: "Java知识点：Optional类的使用"
category: Java
tags: Java Java基础 Java知识点
excerpt: Java类Optional的使用说明。
author: Eric Zong
updated: 2020-08-25 22:27:00
---

* content
{:toc}

# 简介

`Optional` 是 Java 8 新增的一个工具类。它是一个容器，用以包装一个值对象。

使用 `Optional` 可以大大消除充斥于代码中的“判空”操作，使代码更加优雅。

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
更进一步，`Optional` 方法所返回的“空对象”通常也都是这个“缓存空对象“。

`of()` 用于创建一个“非空”的 `Optional` 对象，因此，如果传入 `null` 将会抛出“空指针”。

`ofNullable()` 顾名思义，它可以创建一个任意的 `Optional` 对象，即使它是“空”的。但是，如果传入 `null` 参数则还是会返回“缓存空对象”，而不会另创建一个“空对象”。

# 获取值

`Optional` 包装了一个值，我们最终的目的必定是使用这个值，因此，总是需要提供获取该值的方法：

```java
var value = optional.get();
```

需要注意的是，直接调用 `get()` 方法是有风险的。如果 `optional` 是一个“空对象”，那么，调 `get()` 方法将抛出一个 `NoSuchElementException`。

# 常规判断

有时我们需要根据值是否为“空”要决定如何使用该值，所以，需要事先判断：

```java
optional.isEmpty();
optional.isPresent();
```

# 函数式条件选择

“判断-行为”模式是命令式编程方式，如今 Java 逐渐加入了很多函数式编程能力，而 `Optional` 就有很多这样方法。

对于“值”而言，最常见的一种处理行为是：如果不为空就取其值，否则取另一个值（通常意义上的“默认值”）。

为此，`Optional` 提供了以下 3 个方法来实现该功能：

```java
T orElse(T other)
T orElseGet(Supplier<? extends T> supplier)
Optional<T> or(Supplier<? extends Optional<? extends T>> supplier)
```

最简单直接的就是 `orElse()`，调用该方法的 `Optional` 对象包装的“非空值”时，直接返回该“值”，否则返回参数给定的“默认值”。

进一步地，如果这个“默认值”不是现成的，需要一些计算呢？更复杂一点，如果这些计算很耗时，想要“延迟计算”呢？那么，就需要使用 `orElseGet()` 。

再进一步，如果想要“默认值”也以 `Optional` 对象的形式返回呢？使用 `or()` 方法。这可能常见于“链式编程”中。

`orElseGet()` 和 `or()` 方法的参数都是 `Supplier`，这要求使用者应当了解 lambda 表达式等知识点，这里就不展开讨论了。给一些示例：

```java
Optional.empty().orElse("default"); // default
Optional.of("value").orElse("default"); // value

Optional.empty().orElseGet(() -> "default"); // default
Optional.of("value").orElseGet(() -> "default"); // value

Optional.empty().or(() -> Optional.of("default")); // default
Optional.of("value").or(() -> Optional.of("default")); // Optional[value]
```

在某些情况下，不应得到一个“默认值”，而是应该抛出异常，报告错误。这就得用到以下两个方法了：

```java
T orElseThrow()
<X extends Throwable> T orElseThrow(Supplier<? extends X> exceptionSupplier)
```

`orElseThrow()` 方法在“空值”情况下会抛出 `NoSuchElementException`。

> 这与 `get()` 方法的行为一模一样，不是么？是的，甚至它们的源码都是一样的。
>
> 那为什么要写两个方法？仔细看 API 文档，`orElseThrow()` 是 Java 10 才加入的。`get()` 方法的注释说明了其最佳推荐替代方法就是 `orElseThrow()`。
>
> 为什么要加一个方法替代呢？笔者推测，这里应该是出于语义的考量。`get` 字面上就是“取值”的意思，在现实语义中，如果取的值不存在，通常我们说“不存在”，而不是说“这是一个错误”。因此，`get()` 的行为与现实语义有差异，存在误用的可能。而 `orElseThrow()` 字面就强调了“抛出异常”。
>
> 编程中，应尽量使用 `orElseThrow()`  而非 `get()`。

“值”的另一种处理行为是：如果不为空就以某种方式处理，或者为空则以另一种方式处理。这对应以下两个方法：

```java
void ifPresent(Consumer<? super T> action)
void ifPresentOrElse(Consumer<? super T> action, Runnable emptyAction)    
```

显然，`ifPresent()` 只处理值存在的情况，而 `ifPresentOrElse()` 不论值是否存在都提供了处理方式。

```java
Optional.empty().ifPresent(value -> System.out.println(value)); // 无输出
Optional.of("value").ifPresent(value -> System.out.println(value));  // value

Optional.empty().ifPresentOrElse(value -> System.out.println(value), 
  () -> System.out.println("null")); // null
Optional.of("value").ifPresentOrElse(value -> System.out.println(value), 
  () -> System.out.println("null")); // value
```

# 数据处理

`Optional` 还提供了数据映射和过滤的能力，由以下 3 个方法提供：

```java
<U> Optional<U>	map(Function<? super T, ? extends U> mapper)
<U> Optional<U>	flatMap(Function<? super T, ? extends Optional<? extends U>> mapper)
Optional<T> filter(Predicate<? super T> predicate)
```

`map()` 和 `flatMap()` 区别在于，前者会将值自动包装为 `Optional` 而后者需要自行包装返回。而 `filter()` 用于过滤 `Optional`。

```java
Optional.empty().map(e -> "value"); // Optional.empty
Optional.of("value").map(e -> "other"); // Optional[other]

Optional.empty().flatMap(e -> Optional.of("value")); // Optional.empty
Optional.of("value").flatMap(e -> Optional.of("other")); // Optional[other]

empty.filter(e -> e == null); // Optional.empty，保留，但值为“空”
empty.filter(e -> e != null); // Optional.empty，过滤，结果为“空”
value.filter(e -> e == null); // Optional.empty，过滤
 value.filter(e -> e != null); // Optional[value]，保留
```

# 不可避免的空指针

`Optional` 可以更为“优雅”地处理“空值”，而不用频繁判断。但是，这并不代表其所有方法都不会抛出空指针异常，以下调用还是会导致空指针的：

```java
// empty 代表一个“空”Optional
// value 代表一个“非空”Optional
// optional 代表任意的 Optional
Optional.of(null);
empty.orElseGet(null);
empty.or(() -> null);
optional.or(null);
value.ifPresent(null);
empty.ifPresentOrElse(value -> System.out.println("non-null"), null);
value.ifPresentOrElse(null, () -> System.out.println("null"));
optional.map(null);
optional.flatMap(null);
value.flatMap(e -> null);
optional.filter(null);
```

# 异常可不止空指针

关于异常，还值得注意的是，它可能还会抛出 `NoSuchElementException`。该异常意味着，试图获取一个“空对象的值”。显然，只有 `get()` 和 `orElseThrow()` 这两个取值方法可能导致该异常。

```java
// NoSuchElementException
empty.get();
empty.orElseThrow();
```

# 小结

使用 `Optional` 的优点之一就是不用关注空值，而只用关注处理逻辑，因此，代码具有一致性。

当然，`Optional` 可以进行链式编程，进行一系列的值处理。

总之，`Optional` 为值处理提供了“确定性”，只要正确使用其“可信”的方法，其结果就是稳定可期的。

# 参考

* [Java API - Optional](https://docs.oracle.com/en/java/javase/13/docs/api/java.base/java/util/Optional.html)
* [菜鸟教程 - Java 8 函数式接口](https://www.runoob.com/java/java8-functional-interfaces.html)

