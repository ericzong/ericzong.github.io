---
layout: page
title: Maven参考
categories: Java
---

* content
{:toc}

# 依赖范围与classpath关系

| 依赖范围（Scope） | 编译 | 测试 | 运行时 | 例子         |
| ----------------- | ---- | ---- | ------ | ------------ |
| compile           | Y    | Y    | Y      |              |
| test              | -    | Y    | -      |              |
| provided          | Y    | Y    | -      | servlet-api  |
| runtime           | -    | Y    | Y      | JDBC驱动实现 |
| system            | Y    | Y    | -      | 本地类库文件 |

# 传递性依赖和依赖范围

|          | compile  | test | provided | runtime  |
| -------- | -------- | ---- | -------- | -------- |
| compile  | compile  | -    | -        | runtime  |
| test     | test     | -    | -        | test     |
| provided | provided | -    | provided | provided |
| runtime  | runtime  | -    | -        | runtime  |

# 远程仓库配置

## updatePolicy元素

| 候选值          | 说明             |
| --------------- | ---------------- |
| daily (default) | 每天检查一次更新 |
| never           | 从不更新         |
| always          | 每次构建更新     |
| interval        | 每隔X分钟更新    |

## checksumPolicy元素

| 候选值         | 说明               |
| -------------- | ------------------ |
| warn (default) | 构建时输出警告信息 |
| fail           | 构建失败           |
| ignore         | 忽略校验和错误     |

# 生命周期

| 生命周期 | 阶段                    | 内置插件绑定                         | 说明                   |
| -------- | ----------------------- | ------------------------------------ | ---------------------- |
| clean    |                         |                                      | 清理项目               |
|          | pre-clean               |                                      |                        |
|          | clean                   | maven-clean-plugin:clean             | 清理上次构建生成的文件 |
|          | post-clean              |                                      |                        |
| default  |                         |                                      | 构建项目               |
|          | validate                |                                      |                        |
|          | initialize              |                                      |                        |
|          | generate-sources        |                                      |                        |
|          | process-sources         |                                      |                        |
|          | generate-resources      |                                      |                        |
|          | process-resources       | maven-resources-plugin:resources     |                        |
|          | compile                 | maven-compiler-plugin:compile        |                        |
|          | process-classes         |                                      |                        |
|          | generate-test-sources   |                                      |                        |
|          | process-test-sources    |                                      |                        |
|          | generate-test-resources | maven-resources-plugin:testResources |                        |
|          | process-test-resources  |                                      |                        |
|          | test-compile            | maven-compiler-plugin:testCompile    |                        |
|          | process-test-classes    |                                      |                        |
|          | test                    | maven-surefire-plugin:test           |                        |
|          | prepare-package         |                                      |                        |
|          | package                 | maven-jar-plugin:jar                 |                        |
|          | pre-integration-test    |                                      |                        |
|          | integration-test        |                                      |                        |
|          | post-integration-test   |                                      |                        |
|          | verify                  |                                      |                        |
|          | install                 | maven-install-plugin:install         |                        |
|          | deploy                  | maven-deploy-plugin:deploy           |                        |
| site     |                         |                                      | 建立项目站点           |
|          | pre-site                |                                      |                        |
|          | site                    | maven-site-plugin:site               |                        |
|          | post-site               |                                      |                        |
|          | site-deploy             | maven-site-plugin:deploy             |                        |

> 注：default 生命周期的阶段列出的是 jar 打包方式时的绑定。其他打包方式时的绑定详见[这里](http://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#Built-in_Lifecycle_Bindings)。

# 反应堆裁剪选项

| 选项 | 全称                  | 说明                           |
| ---- | --------------------- | ------------------------------ |
| -am  | --also-make           | 同时构建所列模块的依赖模块     |
| -amd | -also-make-dependents | 同时构建依赖于所列模块的模块   |
| -pl  | --projets \<arg\>     | 构建指定模块，模块间用逗号分隔 |
| -rf  | -resume-from \<arg\>  | 从指定模块回复反应堆。         |

# 测试

测试类默认模式：

1. \*\*/Test\*.java
2. \*\*/\*Test.java
3. \*\*/\*TestCase.java

# Maven 属性

| 属性类型     | 示例                                        |
| ------------ | ------------------------------------------- |
| 内置属性     | \{basedir\}, \{version\}                |
| POM 属性     |                                             |
| 自定义属性   |                                             |
| Settings属性 | \{settings.localRepository\} （本地仓库） |
| Java系统属性 | \{user.home\}，通过 mvn help:system 查询  |
| 环境变量属性 | \{env.JAVA_HOME\}                         |

# 激活 profile

1. 命令行激活。mvn clean install -Pdev-x,dev-y
2. settigns 文件显式激活。
3. 系统属性激活。分为存在性激活和值匹配激活。注意，系统属性可通过命令行传入。
4. 操作系统环境激活。
5. 文件存在性激活。
6. 默认激活。

有任何一个 profile 通过默认激活外的其他任意一种方式被激活，所有默认激活配置失效。

详见[pom.xml配置]({{site.url}}/page/maven-settings-reference#profile)和[settings.xml配置]({{site.url}}/page/maven-settings-reference#profile-1)。

# 命令参考

```shell
# 生成项目骨架
mvn archetype:generate
# 查看已解析依赖（Resolved Dependency）
mvn dependency:list
# 查看依赖树
mvn dependency:tree
# 分析依赖
mvn dependency:analyze
# 清理编译部署
mvn clean deploy site-deploy
# 跳过测试
mvn package -DskipTests
# 跳过测试代码编译（跳过编译也就不会执行测试）
mvn package -Dmaven.test.skip=true
# 动态指定运行的测试用例，可使用通配符（*）及用逗号（,）分隔多个测试类
mvn test -Dtest=XxxTest
# 激活 profile
mvn clean install -Pdev-x,dev-y
# 查看当前激活的 profile
mvn help:active-profiles
# 查看当前所有 profile
mvn help:all-profiles
```

> 分析依赖的命令结果中主要有两个部分。
>
> Used undeclared dependencies，项目中使用到但未显式声明的依赖。使用主要指 import 导入等，通过直接依赖传递引入，升级直接依赖时可能导致问题。
>
> Unused declared dependencies，项目中未使用但显式声明的依赖。理论上可以删除，但通常这些依赖是运行时所需的，删除前需要仔细地分析。

# 杂项

超级 POM 位置

$MAVEN_HOME/lib/maven-model-builder-x.x.x.jar!/org/apache/maven/model/pom-4.0.0.xml。