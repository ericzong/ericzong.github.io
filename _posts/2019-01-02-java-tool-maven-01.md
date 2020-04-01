---
layout: post
title: "Maven笔记：1 入门"
categories: Java
tags: Java Java工具
excerpt: Maven入门
author: "Eric Zong"
---

* content
{:toc}

# 概述

Maven 是一个跨平台项目管理工具，主要用于基于 Java 的项目构建、依赖管理和项目信息管理。

在 Maven 中，所有的依赖项，甚至项目本身都是“构件”，并使用坐标唯一标识一个构件。

坐标，是每个构件的唯一标识。其元素包括：groupId、artifactId、version、packaging、classifier。

| 坐标元素   | 定义       | 说明           | 示例                                                |
| ---------- | ---------- | -------------- | --------------------------------------------------- |
| groupId    | 必需       | 构件所属项目名 |                                                     |
| artifactId | 必需       | 构件所属模块名 |                                                     |
| version    | 必需       | 版本           |                                                     |
| packaging  | 可选       | 打包方式       | jar（默认）, war...                                 |
| classifier | 不直接定义 | 附属构件       | javadoc, sources, jdk15（testNG 5.8 的 Java5 版本） |

> classifier 不直接定义，因为附属构件不是项目直接默认生成的，而是由附加插件帮助生成的。

Maven 项目核心是 pom.xml，POM（Project Object Model，项目对象模型）。

Maven 项目优点之一是解耦/正交性，指项目对象模型最大程度地与实际代码相互独立。

# 关于依赖

依赖范围，用以指定构件在哪些 classpath 中有效。由 \<scope\> 元素定义。被依赖的构件可在 3 个 classpath 中使用，分别是编译主代码、编译测试代码、运行时。（参考[这里]({{site.url}}/references/Maven-reference#%E4%BE%9D%E8%B5%96%E8%8C%83%E5%9B%B4%E4%B8%8Eclasspath%E5%85%B3%E7%B3%BB)）

传递性依赖，即间接依赖。构件A依赖构件B，而构件B依赖构件C，则A对C即为传递依赖。传递依赖会影响依赖范围（参考[这里]({{site.url}}/references/Maven-reference#%E4%BC%A0%E9%80%92%E6%80%A7%E4%BE%9D%E8%B5%96%E5%92%8C%E4%BE%9D%E8%B5%96%E8%8C%83%E5%9B%B4)）。

依赖调解（Dependency Mediation），当传递依赖出现同一构件的不同版本时的选择机制。其原则是：路径最近者优先；第一声明者优先。

可选依赖，非必须可选依赖。由 \<optional\> 元素定义。不传递。典型应用场景是数据库驱动程序，多个驱动通常不需同时使用，只使用某一种，则驱动依赖应定义为可选的。

# 生命周期

Maven 定义了三套[生命周期]({{site.url}}/references/Maven-reference#%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)，每个生命周期又分为多个阶段，每个阶段定义了要完成的工作。而每个阶段的具体工作是由插件来完成的，一些常用的阶段内置绑定了默认的工作插件。插件以构件的形式存在，在需要时下载使用。

各个生命周期是相互独立的，而一个生命周期的阶段是前后依赖关系。因此，我们可以指定每个生命周期执行到哪个阶段，比如：

```shell
mvn clean deploy site-deploy
```

插件本身为了代码复用，通常能支持多个任务，这样的一个插件功能称为“插件目标（Plugin Goal）”。Maven 为一些生命周期的阶段绑定了默认的插件目标，具体可参考[这里]({{site.url}}/references/Maven-reference#%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)。

 default 生命周期的阶段与插件目标的绑定关系由项目打包类型所决定。

当多个插件目标绑定到同一个阶段的时候，这些插件声明的先后顺序决定了目标的执行顺序。

插件的命令行参数是由插件参数的表达式（Expression）决定的，如果没有则参数只能在 POM 中配置。

# 仓库

本地仓库的布局：groupId/artifactId/version/artifactId-version.packaging。

# 聚合与继承

由于大型的项目通常会按模块开发，为了构建或管理等方面的便利，因此，会使用聚合和继承。

相关配置参考 [聚合配置]({{site.url}}/references/maven-settings-reference#%E8%81%9A%E5%90%88) 和 [继承配置]({{site.url}}/references/maven-settings-reference#%E7%BB%A7%E6%89%BF)。

## 聚合

聚合模块是帮助聚合其他模块构建的工具，它提供的是其他被聚合模块的信息，本身没有代码。

聚合模块和被聚合模块共同组成的构建结构被称为“反应堆（Reactor）”。

在聚合模块上执行构建时，模块间的依赖关系会将反应堆构成一个有向非循环图（Directed Acyclic Graph, DAG），根据它可逐一构建各个模块，这个构建顺序称为“反应堆构建顺序（Reactor Build Order）”。

## 继承

同一项目各模块间所使用的依赖和插件版本应是统一一致的，因此，通常创建一个父模块统一管理，以消除配置重复。

父模块主要包含统一的配置信息，不包含代码，甚至可以不是一个项目，仅以父 POM 文件形式被引用即可。默认父 POM 在上一层目录下。

## 比较

聚合和继承的区别在于：

|      | 聚合                              | 继承                        |
| ---- | --------------------------------- | --------------------------- |
| 目的 | 方便快速构建                      | 消除重复配置                |
| 关联 | 聚合模块 → 被聚合模块（单向关联） | 父模块 ← 子模块（单向关联） |

但它们之间也有相似之处，比如：它们的打包方式都应配置为 packaging；除 POM 外无实质内容。甚至，它们常常组合在一起使用，即聚合模块同时也管理被聚合模块的依赖、插件等配置。

# 测试

测试通过插件 maven-surefire-plugin 实现。可通过配置或命令行参考设置其具体测试行为。

## 跳过

Maven 支持跳过测试，甚至是跳过测试代码编译。这些都可以在命令行或是 POM 中设置，详见 [命令参考]({{site.url}}/references/Maven-reference#%E5%91%BD%E4%BB%A4%E5%8F%82%E8%80%83) 和 [配置参考]({{site.url}}/references/maven-settings-reference#%E8%B7%B3%E8%BF%87%E6%B5%8B%E8%AF%95)。