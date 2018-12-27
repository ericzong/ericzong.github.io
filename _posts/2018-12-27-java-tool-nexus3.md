---
layout: post
title: Nexus3应用笔记
categories: Java
tags: Java Java工具
excerpt: 介绍Nexus3的安装及基本使用。
author: "Eric Zong"
---

* content
{:toc}

# 前言

本文主要介绍 Nexus Repository Manager OSS 3.x 在 Windows 环境下的安装配置。

# 准备

下载 Nexus 安装包：https://www.sonatype.com/download-oss-sonatype

安装 JDK，对于 Nexus 3.x 而言，至少应该安装 Java 8。

# 安装

解压下载到的 Nexus 压缩文件到任意文件夹下，会得到两个文件夹：nexus-xxx 和 sonatype-work。

进入 nexus-xxx 的 bin 目录，打开命令行并执行如下命令：

```
nexus.exe /run
```

启动日志出现“Started Sonatype Nexus”时启动完成。

> 可选的命令有：start、stop、run、run-redirect、status、restart、force-reload。
>
> 注意在 Windows 命令行中，所有命令都应有前缀“/”。

当然，我们也可以把 Nexus 安装为 Windows 服务：

```
nexus.exe /install <optional-service-name>
```

这样就可以使其以 Windows 服务的方式自启动了，默认服务名为“nexus”。

Nexus 启动成功后，即可通过 http://localhost:8081/ 访问。

第一次进入 Nexus 管理页面时，应该使用默认的管理员帐号（admin/admin123）登录，并修改密码。

# 配置 

\<nexus\>/bin/nexus.vmoptions 可对 Nexus 的 JVM 参数进行一些配置，主要可以配置下内存参数。

```properties
# 内存分配
-Xms256M
-Xmx1200M
# 数据目录
-Dkaraf.data=../sonatype-work/nexus3
-Djava.io.tmpdir=../sonatype-work/nexus3/tmp
-XX:LogFile=../sonatype-work/nexus3/log/jvm.log
```

\<sonatype-work\>\nexus3\etc\nexus.properties 可配置端口、内容路径等参数。

```properties
# 端口
application-port=8888
# 内容路径
nexus-context-path=/components/
```

> 注意，上面两个参数会改变 Nexus 的访问路径，如果按上面的配置，最终的访问路径应该是：localhost:8888/components/。

# 管理

成功部署并启动 Nexus 后，以管理员权限登录，导航到“Repositories”管理页面，会看到 Nexus 默认已经创建了一些仓库。

跟 Maven 相关的应该有 4 个：maven-central、maven-public、maven-releases、maven-snapshots。

这些仓库的类型各不相同，这里需要简单介绍下仓库类型相关知识。

| 仓库类型 | 描述                   | 用法                                                    |
| -------- | ---------------------- | ------------------------------------------------------- |
| proxy    | 代理访问公网的中央仓库 | maven-central，获取中央仓库构件                         |
| hosted   | 私有仓库               | maven-releases 和 maven-snapshots，发布管理内部构件     |
| group    | 聚合仓库               | maven-public，可访问所包含的所有仓库资源，供 Maven 使用 |

通常，使用默认仓库的仓库也是可以的，但 maven-central 链接的是 https://repo1.maven.org/maven2/ ，速度上可能会不太稳定。

因此，可参考 maven-central 创建一个代理仓库链接阿里的 Maven 仓库（http://maven.aliyun.com/nexus/content/groups/public/）——比如，取名为 maven-ali。

创建好 maven-ali 仓库后，需要我们手动将其添加到 maven-public 聚合仓库中，并将其移动到 maven-central 之前，以便优先使用。最终，聚合仓库中各成员仓库的顺序应该是：maven-releases、maven-snapshots、maven-ali、maven-central。

# Maven 配置

我们希望 Maven 使用上面搭建的私服，所以需要对 Maven 进行配置，主要需要修改配置文件 settings.xml。

## 镜像

所有访问都应通过私服，所以需要配置一个镜像节点。

```xml
<mirror>
	<id>nexus-mirror</id>
	<mirrorOf>*</mirrorOf>
	<name>Nexus Mirror</name>
	<url>http://localhost:8888/components/repository/maven-public/</url>
</mirror>
```

## profile

配置 \<profile\> 节点，以指定私服地址。

```xml
<profile>
	<id>nexus-local</id>
	<repositories>
		<repository>
			<id>nexus</id>
			<url>http://localhost:8888/components/repository/maven-public/</url>
			<releases>
				<enabled>true</enabled>
			</releases>
			<snapshots>
				<enabled>true</enabled>
			</snapshots>
		</repository>
	</repositories>
	
	<pluginRepositories>
		<pluginRepository>
			<id>nexus</id>
			<url>http://localhost:8888/components/repository/maven-public/</url>
			<releases>
				<enabled>true</enabled>
			</releases>
			<snapshots>
				<enabled>true</enabled>
			</snapshots>
		</pluginRepository>
	</pluginRepositories>
</profile>
```

并配置激活 profile，注意 \<activeProfile\> 中配置的是上面 \<profile\> 节点的 \<id\>：

```xml
<activeProfiles>
    <activeProfile>nexus-local</activeProfile>
</activeProfiles>
```

## 发布配置

由于我们还会发布内部构件，所以需要添加发布相关的节点信息。

```xml
<server>
	<id>nexus-releases</id>
	<username>zonglu</username>
	<password>xxx</password>
</server>
<server>
	<id>nexus-snapshots</id>
	<username>zonglu</username>
	<password>xxx</password>
</server>
```

还需要在项目 pom.xml 配置文件中添加发布配置：

```xml
<distributionManagement>
    <repository>
        <id>nexus-releases</id>
        <name>Nexus Release Repository</name>
        <url>http://localhost:8888/components/repository/maven-releases/</url>
    </repository>
    <snapshotRepository>
        <id>nexus-snapshots</id>
        <name>Nexus Snapshot Repository</name>
        <url>http://localhost:8888/components/repository/maven-snapshots/</url>
    </snapshotRepository>
</distributionManagement>
```

注意，repository.id 和 snapshotRepository.id 是跟 settings.xml 中 server.id 一一对应的。否则会因找不到对应的授权信息而发布失败。