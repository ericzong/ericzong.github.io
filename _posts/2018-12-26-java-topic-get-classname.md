---
layout: post
title: Java专题：获取类名
categories: Java
tags: Java Java专题
excerpt: 比较各种类名获取方法的差异。
author: "Eric Zong"
---

* content
{:toc}

# 获取类名的方法

* Class.getName()
* Class.getCanonicalName()
* Class.getSimpleName()

# 差异对比

|          | getName()                         | getCanonicalName()               | getSimpleName() |
| -------- | --------------------------------- | -------------------------------- | --------------- |
| 普通类   | pkg.ClassName                     | pkg.ClassName                    | ClassName       |
| 嵌套类   | pkg.EnclosingClass$NestedClass    | pkg.EnclosingClass.NestedClass   | NestedClass     |
| 局部类   | pkg.EnclosingClass$nLocal         | null                             | LocalClass      |
| 匿名类   | pkg.EnclosingClass$n              | null                             | 空字符串        |
| 普通数组 | [Lpkg.ClassName;                  | pkg.ClassName[]                  | ClassName[]     |
| 嵌套数组 | [Lpkg.EnclosingClass$NestedClass; | pkg.EnclosingClass.NestedClass[] | NestedClass[]   |
| 局部数组 | [Lpkg.EnclosingClass$nLocalClass; | null                             | LocalClass[]    |
| 匿名数组 | [Lpkg.EnclosingClass$n            | null                             | []              |

小结：

* 对 getName() 而言，主要特殊之处在于：嵌套类使用“$”分隔外围类与嵌套类；数组使用 JNI 字段描述符形式
* 对 getCanonicalName() 而言，主要特殊之处在于：局部类、匿名类和局部类数组的规范名为 null
* 对 getSimpleName() 而言，主要特殊之处在于：匿名类简单名为空（字符串）

# 附录

## JNI字段描述符(JavaNative Interface FieldDescriptors)

| Java 类型 | JNI 字段描述符                                               |
| --------- | ------------------------------------------------------------ |
| boolean   | Z                                                            |
| byte      | B                                                            |
| char      | C                                                            |
| short     | S                                                            |
| int       | I                                                            |
| long      | J                                                            |
| float     | F                                                            |
| double    | D                                                            |
| void      | V                                                            |
| class     | Lclassname;<br/>（以“L”开头，“;”结尾；路径分隔符“/”；嵌套类用“$”分隔；数组前缀“[”，个数代表深度） |

注意：

getName() 返回的 JNI 字段描述符中，路径分隔符是“.”。

## 匿名类及其数组

“获取类名”和“匿名类”从字面逻辑上来看就是相悖的，就像“获取匿名类的类名”这句话一样听起来很怪。

但正如前文表格列出的一样，这是合法的。

### 匿名类的特殊性

在本文中，匿名类显得尤为特殊，因为本文在说明获取类名的方法，而匿名类理应是没有名字的。但匿名类的特殊性不仅如此，还包括：

1. 无构造器；
2. 声明的同时创建实例；
3. 其类型无法通过类型字面量引用；
4. 无法声明该类型的变量及数组变量；
5. 无法“常规”创建该类型数组。

### 匿名类实例唯一吗？

先说答案：不一定。

首先，由于匿名类实例是在匿名类声明的同时创建，而匿名类无法通过类型字面量访问，换句话说，不可能再 new 一个同类型实例。

其次，匿名类没有构造器，因此，不可能通过反射调用其构造器实例化。

但是，还有两种创建对象的方式：克隆和反序列化，但它们都是有相应限制的。

如果匿名类的父类型是 Cloneable 的，那么就可以通过克隆创建另一个同类型实例。

至于序列化，限制要更多一点，除了要求父类型是 Serializable 之外，还要求匿名类的外围类也是 Serializable 的。应注意到，匿名类是否可序列化取决于两方面：

1. 外围类是否可序列化；
2. 自身是否可序列化，亦即其父类型是否可序列化。

总结来说，除非匿名类是 Cloneable 或其父类型及外围类均是 Serializable 的，否则该匿名类将仅有唯一实例。

### 不能“常规”创建数组

由于不能通过类型字面量引用匿名类，意即不能以 `Xxx.class` 的方式访问其类类型，因此需要调用其实例对象的 `getClass()` 方法来访问。

另一方面说明，我们不得不声明匿名类的父类型的变量来持有其实例，比如：`Object o = new Object() {}` 。进一步来说，如果创建了一个匿名类数组也只能赋值给其父类型的数组。

那么，可以创建一个匿名类数组吗？

也许，有人会想 `Object[] array = new Object[] {}` 就能创建匿名类数组，遗憾地是这仅仅是创建了一个 Object 数组。

因此，目前来看，至少我们不能以“常规”的方式来创建匿名类数组。

有没有其他方式来创建呢？答案是有的。通过 Array 类相关实例化方法就可以创建一个匿名类数组：

```java
Data d = new Data() {};
Data[] array = (Data[]) Array.newInstance(d.getClass(), 10);
// array.getName(): [Lcom.ericzong.java.sample.api.ClassNameTest$4;
// array.getCanonicalName(): null
// array.getSimpleName(): []
```

### 小结

由于无名，所以导致匿名类在与名称相关的方面都比较特别。

无名 →  无构造器 → 必需在声明时实例化；

无名 → 无类型字面量 → 无法声明（数组）变量

​                                    → 无法“常规”创建数组。

最后，想要说的是，虽然可能创建多个同一匿名类的实例，也可以创建匿名类数组，但说实话，笔者并未想到其实际应用场景，也没有合理的理由让我们非得这样编码。

