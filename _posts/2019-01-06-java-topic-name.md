---
layout: post
title: "Java专题：名字及其重用"
categories: Java
tags: Java Java专题
excerpt: 讨论Java名字及其重用。
author: "Eric Zong"
---

* content
{:toc}

# 基本概念

## 定义

名字：用来引用在程序中声明实体的标识符（序列）。

名字分为：简单名和限定名。

* 简单名：由单个标识符构成。
* 限定名：由标识符序列构成；（递归定义）由一个名字、一个“.”符号和一个标识符构成。

## 说明

消除名字二义性是通过使用名字出现位置的上下文来实现的。

名字是标识符，但标识符不一定是名字。

> 名字与标识符的区别有一定的复杂性，不太易于理解，这里简单说明下，详细参见《Java 语言规范》。
>
> * 在被声明时，标识符仅仅是标识符；
> * 域访问表达式、方法调用表达式、方法引用表达式、限定的类实例创建表达式，使用标识符；
> * Array.length 是限定名。
>
> 标号语句的标号（即俗称的“标签”）只是标识符，由于标号与名字使用的上下文不存在交集，所以即使它们同名也不会相互遮掩。
>

# 名字重用

## 覆写（override）

一个实例方法可以覆写在其超类中可访问到的具有相同签名的所有实例方法，从而使能了动态分派（dynamic dispatch）；

换句话说，VM 将基于实例的运行期类型来选择要调用的覆写方法。

覆写是面向对象编程技术的基础，并且是唯一没有被普遍劝阻的名字重用形式。

## 隐藏（hide）

隐藏，只作用于本应继承但因子类中的声明而没有被继承的成员。

一个域、静态方法或成员类型可以分别隐藏在其超类中可访问到的具有相同名字（对方法而言就是相同的方法签名）的所有域、静态方法或成员类型。

隐藏一个成员将阻止其被继承。

## 重载（overload）

在某个类中的方法可以重载另一个方法，只要它们具有相同的名字和不同的签名。由调用所指定的重载方法是在编译期选定的。

## 遮蔽（shadow）

一个声明只能遮蔽类型相同的另一个声明。即一个变量、方法或类型可以分别遮蔽在一个闭合的文本范围内的具有相同名字的所有变量、方法或类型。

如果一个实体被遮蔽了，那么用它的简单名是无法引用到它的；根据实体的不同，有时根本就无法引用到它。

包声明永远不会遮蔽其他声明；按需类型导入声明、按需静态导入声明永远都不会遮蔽其他声明。

局部变量、形参、异常参数、局部类都只能用简单名引用，因此，它们通常不能被遮蔽，否则将不能被引用到，故是一个编译时错误。

一个例外情况是，可以被嵌套类型中的对应实体名字所遮蔽。

添加一个私有类声明，由于遮蔽，可能间接地修改了一个现有公共方法的返回类型，而这是一个不兼容的API修改，因为我们修改了一个被导出 API 所使用的名字的含义。

尽管遮蔽通常是被劝阻的，但是有一种通用的惯用法确实涉及遮蔽。构造器经常将来自其所在类的某个域名重用为一个参数，以传递这个命名域的值。

这种惯用法并不是没有风险，但是大多数Java程序员都认为这种风格带来的实惠要超过其风险。

## 遮掩（obscure）

一个声明遮掩类型不同的另一个声明。变量声明可以遮掩类型和包声明，而类型声明也可以遮掩包声明。（优先级为：变量名 > 类型名 > 包名）

本身就属于某个范围的成员在该范围内与静态导入相比具有优先权。

>后果之一是与 Object 的方法具有相同名字的静态方法不能通过静态导入而得到使用。

遮掩是唯一一种两个名字位于不同的名字空间的名字重用形式，这些名字空间包括：变量、包、方法或类型。

```
包名被域声明遮掩，使用import。
包名或域名被参数或局部变量声明遮掩，可修改参数或局部变量名。
如果一个类型或一个包被遮掩了，那么你不能通过其简单名引用到它，除非是在这样一个上下文环境中，即语法只允许在其名字空间中出现一种名字。
遵守命名习惯就可以极大地消除产生遮掩的可能性。
```

# 完全限定名与规范名

简单类型（基本数据类型）、具名包、顶层类、顶层接口都有完全限定名和规范名；成员类、成员接口、数组类型可以有。

简单来说，大部分情况下，它们是相同的。区别仅存在于嵌套类和继承相关的情况下，比如：

```java
package p;
class C1 { class N1 {} }
class C2 extends C1 {} 
```

p.C1.N1 和 p.C2.N1 都是完全限定名，但只有 p.C1.N1 是规范名。

注意：完全限定名和规范名同 Class.getName() 方法返回的名称是有区别的，主要是体现在嵌套类和数组的表示上。

Java API 中没有方法返回完全限定名（不唯一），但 Class.getCanonicalName() 方法可返回规范名。

# 示例

## 遮蔽

### 遮蔽类型

```java
public class ShadowClass {
    static class String {
        public String(java.lang.String string) {
        }

        @Override
        public java.lang.String toString() {
            return "Just custom String";
        }
    }
    
    public static void main(java.lang.String[] args) {
        String str = new String("Test");
        System.out.println(str);
    }
}
```

### 局部变量遮蔽域

```java
public class ShadowFieldByLocal {
    static int i = 1;

    public static void main(String[] args) {
        int i = 0;
        System.out.println(i); // 0, access local variable;
        System.out.println(ShadowFieldByLocal.i); // 1, access field
    }
}
```

### 内部类变量遮蔽外围类的域

```java
public class ShadowLocalByInternalField {

    private int i = 0;
    
    void test() {
        class InternalClass {
            void print() {
                int i = 1;
                System.out.println(ShadowLocalByInternalField.this.i); // 0, access field
                System.out.println(i); // 1, access local variable
            }
        }
        
        InternalClass in = new InternalClass();
        in.print();
    }
    
    public static void main(String[] args) {
        new ShadowLocalByInternalField().test();
    }
}
```