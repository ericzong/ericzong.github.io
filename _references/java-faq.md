---
layout: page
title: Java开发常见问题汇总
category: Java
date: 2020-04-09
author: Eric Zong
---

* content
{:toc}
# Eclipse: java facet could not be found

使用新版本的 Eclipse——比如，2020-03 (4.15.0)——导入过去的 Maven 项目——比如，使用的是 java facet version 1.8，会有一个 Warning：

> Implementation of project facet java could not be found. Functionality will be limited.

我们可以找到文件 `<project>/.settings/org.eclipse.wst.common.project.facet.core.xml`，其内容类似这样：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<faceted-project>
  <installed facet="java" version="1.8"/>
</faceted-project>
```

使用 Eclipse 新建的项目没有该文件，尝试删除该文件，并刷新工程，Warning 清除。