---
layout: page
title: package.json参考
permalink: /page/package-json
---

* content
{:toc}
# 依赖

## dependencies

生产（production）依赖，在开发及发布到生产环境时均可用。

不应包含测试工具（test harness）或转换器（transpiler）。

## devDependencies

开发（development）依赖，开发阶段可用。

测试工具（test harness）或转换器（transpiler）通常配置在这里。

## peerDependencies

插件依赖，声明依赖的插件及版本。

> 在 npm1 和 npm2 中，会自动安装该依赖；npm3+ 则不会自动安装，而是报告警告。

## bundledDependencies

绑定依赖，打包发布时会将该依赖一起发布。

## optionalDependencies

可选依赖，即使缺失也不会导致安装失败。

# 语义版本

语义版本，Semantic Versioning, semver。

## 格式

```
major.minor.patch
```

* major：主版本号，API 破坏性或非兼容性修改
* minor：次要版本号，向后兼容新功能
* patch：补丁号，向后兼容 bug 修正
* pre-release tags: 3.1.4-beta.2

## 版本范围

### 比较符

| 比较符 | 说明                     | 示例    | 示例范围       |
| ------ | ------------------------ | ------- | -------------- |
| <      | 小于                     | \<1.2.3  | [0.0.0, 1.2.3) |
| <=     | 小于等于                 | \<=1.2.3 | [0.0.0, 1.2.3] |
| >      | 大于                     | \>1.2.3  | (1.2.3, ) |
| >=     | 大于等于                 | \>=1.2.3 | [1.2.3, ) |
| =      | 等于，等价于直接写版本号 | =1.2.3 | 1.2.3 |

## 集合运算

| 集合运算 | 示例               | 示例范围                 |
| -------- | ------------------ | ------------------------ |
| 交集     | \>=1.2.3 \<3.1.4   | [1.2.3, 3.1.4)           |
| 并集     | <1.2.3 \|\| >3.1.4 | [0.0.0, 1.2.3) (3.1.4, ) |

## 连字符（Hyphen）范围



## X 范围



## 波浪号（Tilde）范围



## 脱字号（Caret）范围



# 参考

[npm-semver](https://docs.npmjs.com/misc/semver)

[语义化版本](https://semver.org/lang/zh-CN/)