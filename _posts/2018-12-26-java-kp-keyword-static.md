---
layout: post
title: "Java知识点：static关键字"
categories: Java
tags: Java Java基础 Java知识点
excerpt: Java关键字static相关知识点。
author: "Eric Zong"
---

* content
{:toc}

Java 关键字 static 跟 final 一样，有很多用处，也是“关键字重载”的典范。
一般可用在 5 处：

  1. 静态成员类
  2. 静态方法
  3. 静态变量
  4. 静态初始化块
  5. 静态导入

下面的示例展示了全部的用法：

```java
package com.ericzong.java.sample.syntax;
// 静态导入，可导入静态成员，这里导入的是静态方法
import static com.ericzong.java.sample.syntax.StaticTest.StaticClass.test;

public class StaticTest {

    public static void main(String[] args) {
        test();
    }

    // 静态成员类，嵌套类的一种
    public static class StaticClass {
        public static final int test; // 静态变量

        static { // 静态初始化块
            test = 42;
        }

        public static void test() { // 静态方法
            System.out.println(test);
        }
    }
}
```

示例基本说明了 static 关键字的各种用法，但有些细节还是有必要进一步说明。

# 静态成员类

静态成员类，学术一点的说法是“静态嵌套类”。嵌套类的一种，另一种常见的嵌套类是内部类。它们之间的一个区别是：内部类需要隐式地持有一个外部类引用，而静态嵌套类不需要。

# 静态方法

静态方法，也称“类方法”。

不需要通过实例调用，而只需要通过类调用。

由于不依赖实例，所以其中不能出现对 this、super 及实例成员等的访问。

通常静态方法用于编写工具方法，但一些观点认为，这不是面向对象的编程。

# 静态变量

静态变量，也称“类变量”。所有同类实例共享的变量。

静态方法可以访问任意静态成员，与其声明顺序无关；与静态方法不同，静态变量之间需要有合法的前向引用，换句话说，一个静态变量的赋值表达式中不能出现后续声明的静态变量。

# 静态初始化器

静态初始化器中同样不能访问 this、super 及实例成员等。

# 静态导入

静态导入声明分两种：**单静态导入声明**和**按需静态导入声明**。

与普通导入声明不同，静态导入声明导入的是某个类型中的静态成员。

即使是单静态导入声明，也并不意味着仅仅导入一个静态成员，它可能导入多个同名静态成员。比如，示例中静态导入的 test 同时导入了静态变量 test 和静态方法 test()。

