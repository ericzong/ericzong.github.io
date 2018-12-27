---
layout: post
title: "ActiveJDBC笔记：1 入门"
categories: Java
tags: Java Java框架
excerpt: ActiveJDBC入门。
author: "Eric Zong"
---

* content
{:toc}

# 概述

[ActiveJDBC](http://javalite.io/activejdbc)，简而言之，它是一个轻量级的 Java ORM 框架，是 [JavaLite](http://javalite.io/) 工具框架集中的一员。

它比大多数的 ORM 框架简单易用，比如：

1. 可以自动推断数据表与实体模型的映射关系，不需要配置文件或是注解指定，但也可以使用注解指定；
2. 实体模型编写很简单，不用写 getter/setter 方法，甚至可以是一个空的类等等。

# 基本应用

使用上，ActiveJDBC 非常简单，官网上有一个 [5 分钟入门教程](http://javalite.io/activejdbc#minute-introduction) 详细讲解了其基本的使用方法。

下面我们结合该教程简单说明下，当然，官方教程中的内容不一一赘述，但你应该首先仔细阅读。

ActiveJDBC 的基本开发过程如下：

1. 创建数据库表；
2. 编写表对应的数据模型（需继承自 org.javalite.activejdbc.Model）；
3. 编写数据库操作代码；
4. 数据库信息配置（如果硬编码则不需要）。

官方的入门教程详细例举了 CURD 操作示例，值得注意的是其查询操作，从代码可以看出，它不使用原生 SQL，也不使用类似 HQL 的语言，而是使用将关键字包装为方法、数据包装为方法参数的形式编写（这种形式不仅限于 Java，其他语言中也有类似的框架及用法），就好像这样：

```java
List<Employee> people = Employee.where("department = ? and hire_date > ? ", "IT", hireDate)
    .offset(21)
    .limit(10)
    .orderBy("hire_date asc");
```

另外，其数据模型类必需继承自框架 Model 基类，而且也不必声明属性与表字段映射，ActiveJDBC 本身也不进行这样的映射。如果没有特别的需要，甚至只需要定义一个空的模型类，如下面这样：

```java
public class Person extends Model {}
```

简单看下 Model 类的源码就知道，Model 类提供了许多数据操作方法，因此，严格说来，继承自它的模型类并不是单纯的 POJO。

# 植入（Instrumentation）

创建好数据表，配置好 ActiveJDBC 依赖，并参考示例使用 ActiveJDBC 提代的 API 编写好代码，然后编译运行，会得到如下的异常信息：

```
failed to determine Model class name, are you sure models have been instrumented?
```

信息中提到了 ActiveJDBC 的一个重要操作——植入（[Instrumentation](http://javalite.io/instrumentation)）——官网对其进行了详细说明。

植入是什么意思呢？简单来说，它是一个改写模型类字节码的操作。

为什么需要植入？从上文我们知道，数据库操作方法多是由 Model 提供的，而这些方法大都需要知道当前类，用以推断操作的数据表。但问题是，这些方法大都是静态方法，而由于 Java 语言特性，静态方法不能得知调用它的当前类信息。因此，必需有一种手段能提供（并且是“优雅”地）当前类信息，因此，使用了改写模型类字节码的方式。

官网提供了多种植入的执行方式，包括独立的、基于 Maven 的、基于 Ant 的……

通常我使用 Maven 插件的方式进行植入，只需要按说明配置好 Maven 插件，然后执行如下命令即可：

```
mvn activejdbc-instrumentation:instrument
```

命令正确执行会反馈哪些类被植入了。

# 配置参考

## 依赖

```xml
<dependency>
	<groupId>org.javalite</groupId>
	<artifactId>activejdbc</artifactId>
	<version>${activejdbc.version}</version>
</dependency>
```

## 植入插件

```xml
<plugin>
    <groupId>org.javalite</groupId>
    <artifactId>activejdbc-instrumentation</artifactId>
    <version>${activejdbc.instrumentation.version}</version>
    <executions>
        <execution>
            <phase>process-classes</phase>
            <goals>
                <goal>instrument</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

