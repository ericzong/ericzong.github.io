---
layout: post
title: Maven笔记：2 最佳实践
category: Java
tags: Java Java工具
excerpt: Maven最佳实践
author: Eric Zong
---

* content
{:toc}

# 最佳实践

## 设置 MAVEN_OPTS 环境变量

通常，应该通过 MAVEN_OPTS 环境变量设置 Maven 命令运行时相关 JVM 参数，比如内存参数。

MAVEN_OPTS=-Xms128m -Xmx512m

恰当的内存参数可避免当项目过大时 Maven 命令执行报 java.lang.OutOfMemoryError。

> 不应修改 mvn.bat 或 mvn 脚本文件，否则，升级 Maven 后需要再次修改。

## 使用用户配置

应使用用户配置 ~/.m2/settings.xml。

> 应避免使用全局配置 $M2_HOME/conf/settings.xml ，否则，升级 Maven 后需要再次修改。

## 排除依赖

传递依赖会隐式引入很多依赖，某些情况下，我们不希望使用这些传递依赖，而改由我们自己指定。此时，就需要我们在 \<dependency\> 下通过 \<exclusions\> 子元素定义要排除的传递依赖。当然，排除后还要指定我们希望的替代依赖。

## 归类依赖

通常，一些大型项目是按模块开发的，每个模块就发布为一个构件。这样，依赖中就会有同一个项目不同模块的构件，但它们的版本都是一样。带来的问题是，当我们升级这个依赖项目时，需要替换每个模块的版本，不仅繁琐还可能出错。

因此，通常我们将这种版本号作为属性定义在 \<properties\> 元素下，并使用 ${xxx.version} 的形式引用。这样，在升版时就可以统一修改了。

# FAQ

## 插件传递依赖无效

这是一个警告，典型信息如下：

```
[WARNING] The POM for org.apache.maven:maven-model-builder:jar:3.0 is invalid, transitive dependencies (if any) will not be available, enable debug logging for more details
```

从上下文我们可以知道是哪一个插件引发的问题，但是，这是一个传递依赖的问题，所以如果不查看插件的依赖树就不可能知道是哪里的问题。因此，我们可以执行以下命令：

```shell
mvn dependency:analyze -X
```

从 debug 日志中，可以看到插件的依赖树，因此，可以据此调整依赖的版本。

## 聚合项目版本警告

一个包含多个模块（每个模块一个 Maven 项目）的项目，为了方便打包，一般会创建一个聚合项目把各个模块包含进来，以便统一打包。而且，通常也把这个聚合项目做为父项目。

为了统一且方便地升版本号，一种直观的配置方法是，在聚合项目中将版本号配置为属性，聚合项目版本号以及各模块的父项目版本号都引用该属性。这样，只要修改这个版本号属性就可以统一升版了。

这确实可行，但总是会看到如下警告：

```
[WARNING] 'version' contains an expression but should be a constant.
...
[WARNING] It is highly recommended to fix these problems because they threaten the stability of your build.
[WARNING] For this reason, future Maven versions might no longer support building such malformed projects.
```

从警告可知，Maven 期望版本号是一个常量，因此，这样配置有稳定性问题，并且未来的 Maven 可能不支持。总之，这不是一种推荐的配置方法。

正确的方法应该是使用 [Versions Maven Plugin](http://www.mojohaus.org/versions-maven-plugin/index.html) 进行版本升级。

```shell
# 更新版本
mvn versions:set -DoldVersion=* -DnewVersion=0.1.2-SNAPSHOT -DprocessAllModules=true -DallowSnapshots=true
# 确认修改
mvn versions:commit
# 撤销修改
mvn versions:revert
```

