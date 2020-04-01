---
layout: page
title: Maven配置参考
category: Java
date: 2019-01-02
author: "Eric Zong"
---

* content
{:toc}

# POM 配置

## 生成可运行 jar 包

需要借助 maven-shade-plugin。

```xml
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-shade-plugin</artifactId>
  <version>3.1.1</version>
  <executions>
    <execution>
      <phase>package</phase>
      <goals>
        <goal>shade</goal>
      </goals>
      <configuration>
        <transformers>
          <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
            <mainClass>com.ericzong.maven.App</mainClass>
          </transformer>
        </transformers>
      </configuration>
    </execution>
  </executions>
</plugin>
```

## 自定义绑定插件目标

```xml
<!-- package source code -->
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-source-plugin</artifactId>
  <version>3.0.1</version>
  <executions>
    <execution>
      <id>attach-sources</id>
      <phase>verify</phase>
      <goals>
        <goal>jar-no-fork</goal>
      </goals>
    </execution>
  </executions>
</plugin>
<!-- package java API doc -->
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-javadoc-plugin</artifactId>
  <version>3.0.1</version>
  <configuration>
    <aggregate>true</aggregate>
  </configuration>
  <executions>
    <execution>
      <id>attach-javadocs</id>
      <phase>verify</phase>
      <goals>
        <goal>jar</goal>
      </goals>
    </execution>
  </executions>
</plugin>
```

## 聚合

```xml
<packaging>pom</packaging>

<modules>
  <module>../module1</module>
  <module>../module2</module>
  <module>../module3</module>
</modules>
```

聚合模块核心配置即为 \<modules\> 元素，且打包方式必须为 pom。

聚合模块是帮助聚合其他模块构建的工具，本身无实质内容。

反应堆，所有模块组成的构建结构。根据各模块的相互依赖情况确定反应堆构建顺序（Reactor Build Order）。

## 继承

```xml
<parent>
  <groupId>...</groupId>
  <artifactId>...</artifactId>
  <version>...</version>
  <relativePath>../xxx</relativePath>
</parent>

<dependencyManagement></dependencyManagement>

<build>
  <pluginManagement>
  </pluginManagement>
</build>
```

父模块消除配置重复，不包含除 POM 之外的项目文件。

Maven 默认父 POM 在上一层目录下。

## 跳过测试

```xml
<!-- 跳过测试运行 -->
<plugin>
  <artifactId>maven-surefire-plugin</artifactId>
  <configuration>
    <skipTests>true</skipTests>
  </configuration>
</plugin>
<!-- 跳过测试编译（显然也不会运行） -->
<plugin>
  <artifactId>maven-compiler-plugin</artifactId>
  <configuration>
    <skip>true</skip>
  </configuration>
</plugin>
<plugin>
  <artifactId>maven-surefire-plugin</artifactId>
  <configuration>
    <skip>true</skip>
  </configuration>
</plugin>
```

## 包含及排除测试用例

```xml
<plugin>
  <configuration>
    <includes>
      <include>**/*Tests.java</include>        
    </includes>
    <excludes>
      <exclude>**/*TempTest.java</exclude>
    </excludes>
  </configuration>
</plugin>
```

## profile

```xml
<profiles>
  <profile>
    <id>xxx</id>
    <activation>
      <!-- 自动激活，可选* -->
      <activeByDefault>true</activeByDefault>
    </activation>
    <!-- 自定义属性 -->
    <properties>...</properties>
    <!-- 过滤属性 -->
    <resources>
      <resource>
          <directory>...</directory>
          <filtering>true</filtering>
      </resource>
    </resources>
    <testResources>
      <testResource>
        <directory>...</directory>
        <filtering>true</filtering>
      </testResource>
    </testResources>
  </profile>
  <!-- 系统属性激活 -->
  <profile>
    <activation>
      <property>
        <name>xxx</name>
        <!-- 属性值匹配，可选* -->
        <value>TestEnv</value>
      </property>
    </activation>
  </profile>
  <!-- 操作系统环境激活 -->
  <profile>
   <activation>
     <os>
       <name>Windows XP</name>
       <family>Windows</family>
       <arch>x86</arch>
       <version>5.1.2600</version>
     </os>
   </activation>
  </profile>
  <!-- 文件存在激活 -->
  <profile>
    <activation>
      <file>
        <missing>xxx.xxx</missing>
        <exists>yyy.yyy</exists>
      </file>
    </activation>
  </profile>
</profiles>
```

通过，`mvn clean install -P<profile-id>` 启用。

## 配置 JDK

```xml
<build>
  <plugins>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <configuraion>
      <source>1.8</source>
      <target>1.8</target>
    </configuraion>
  </plugins>
</build>
```

## 替换中央仓库

```xml
<repositories>
  <repository>
    <id>alimaven</id>
    <name>aliyun maven</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
  </repository>
</repositories>
```

# settings.xml 配置

## 本地仓库

```xml
<localRepository>E:/maven</localRepository>
```

## 镜像

```xml
<mirror>
  <id>nexus-mirror</id>
  <mirrorOf>*</mirrorOf>
  <name>Nexus Mirror</name>
  <url>http://localhost:8888/nexus/repository/maven-public/</url>
</mirror>
<!-- 替换中央仓库 -->
<mirror>
  <id>alimaven</id>
  <name>aliyun maven</name>
  <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
  <mirrorOf>central</mirrorOf>
</mirror>
```

\<mirrorOf\> 可以使用通配符等高级配置：

| 配置值       | 说明           |
| ------------ | -------------- |
| *            |                |
| external: *  | 所有非本机仓库 |
| repo1, repo2 | 逗号分隔       |
| *, !repo1    | ! 排除         |

## profile

```xml
<profiles>
  <profile>
    <id>jdk-1.8</id>
    <activation>
      <activeByDefault>true</activeByDefault>
      <jdk>1.8</jdk>
    </activation>
    <properties>
      <maven.compiler.source>1.8</maven.compiler.source>
      <maven.compiler.target>1.8</maven.compiler.target>
      <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
    </properties>
  </profile>
  
  <profile>
    <id>nexus-local</id>
    <repositories>
      <repository>
        <id>nexus</id>
        <url>http://localhost:8888/nexus/repository/maven-public/</url>
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
       <url>http://localhost:8888/nexus/repository/maven-public/</url>
        <releases>
          <enabled>true</enabled>
        </releases>
        <snapshots>
          <enabled>true</enabled>
        </snapshots>
      </pluginRepository>
    </pluginRepositories>
  </profile>
  </profiles>
  <!-- 显示激活 -->
  <activeProfiles>
    <activeProfile>jdk-1.8</activeProfile>
    <activeProfile>nexus-local</activeProfile>
  </activeProfiles>
```

## 发布配置

```xml
<servers>
  <server>
    <id>nexus-releases</id>
    <username>zonglu</username>
    <password>password</password>
  </server>
  <server>
    <id>nexus-snapshots</id>
    <username>zonglu</username>
    <password>password</password>
  </server>
</servers>
```

# 第三方插件

## 覆盖率 JaCoCo

### Maven 插件配置

```xml
<plugin>
  <groupId>org.jacoco</groupId>
  <artifactId>jacoco-maven-plugin</artifactId>
  <version>${jacoco.version}</version>
  <executions>
    <execution>
      <id>prepare-agent</id>
      <goals>
        <goal>prepare-agent</goal>
      </goals>
    </execution>
    <execution>
      <id>report</id>
      <phase>prepare-package</phase>
      <goals>
        <goal>report</goal>
      </goals>
    </execution>
    <execution>
      <id>post-unit-test</id>
      <phase>test</phase>
      <goals>
        <goal>report</goal>
      </goals>
      <configuration>
        <dataFile>target/jacoco.exec</dataFile>
        <outputDirectory>target/jacoco-ut</outputDirectory>
      </configuration>
    </execution>
  </executions>
  <configuration>
    <systemPropertyVariables>
      <jacoco-agent.destfile>target/jacoco.exec</jacoco-agent.destfile>
    </systemPropertyVariables>
  </configuration>
</plugin>
```

### 资源

* [官网](https://www.jacoco.org/)
* [与IntelliJ IDEA集成](https://www.jetbrains.com/help/idea/code-coverage.html)
* [JAVA代码覆盖率工具JaCoCo-原理篇](https://www.aliyun.com/jiaocheng/839260.html)
* [Java 代码覆盖率工具 JaCoCo：实践篇](https://www.aliyun.com/jiaocheng/839261.html)
* [Jacoco 代码覆盖率，监控WEB项目](https://www.aliyun.com/jiaocheng/872529.html)
* [如何使用Jacoco远程统计tomcat服务的代码覆盖率](https://my.oschina.net/91jason/blog/491171)

