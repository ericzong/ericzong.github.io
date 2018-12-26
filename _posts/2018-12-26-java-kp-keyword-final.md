---
layout: post
title: "Java知识点：final关键字"
categories: Java
tags: Java Java基础 Java知识点
excerpt: Java关键字final相关知识点。
author: "Eric Zong"
---

* content
{:toc}

# 用法

Java 中，final 关键字可以在多处使用，可以修饰类、方法、变量，效果都很类似，但又各有差异。可谓是“关键字重载”的典范。

总体来说，final 带有一种“不可变”的意思。其各种用法可参考下表：

| 修饰对象 | 效果 |
|:--|:--|
| 类 | 不能被继承 |
| 方法 | 不能被子类重写 |
| 变量 | 不能重新赋值 |

# 知识点

虽然，看起来用法也不算很多，但整理一下相关知识点，发现还是有不少的。

* 变量
  * 不能重新赋值
  * 初始化
    * 静态变量初始化时机
    * 实例变量初始化时机
    * 局部变量初始化时机
    * 空白 final 
    * 常量
  * 隐式 final
  * 事实 final
  * 写受保护的域
* 方法
* 综合
  * 多线程
  * 兼容性
* 比较
  * final vs. finally vs. finalize()
  * final vs. abstract
  * 变量与方法的“重写”行为

# 说明

## 变量

### 不能重新赋值

final 变量只能赋值一次，对于成员变量而言这是必须的，但对于局部变量不是——可是不赋值就不可使用，因此，这是没有意义的，通常也不合规范。

重新赋值包括：使用赋值号赋值，前/后缀加/减。

### 初始化

关于 final 变量初始化，首先需要知道“空白 final”的概念。空白 final 指声明缺少初始化器的 final 变量。换句话说，如果声明时赋值了，那就不是空白 final。

各种变量都可以声明为空白 final，但其后续赋值时机限制是不同的。

* 对于局部变量而言，只需要在使用该变量前为其赋值即可。

* 对于静态变量而言，需要在类初始化完成前赋值。
* 对于实例变量而言，需要在实例初始化完成前赋值。

因此，静态变量只有 2 个赋值时机：声明时和 static 初始化块中；实例变量有 3 个赋值时机：声明时、普通初始化块中和构造器中。



另外一种常见的特殊的变量是常量变量，也就是我们常说的“常量”。

常量变量指用常量表达式初始化的 final 变量。至于**常量表达式**的准确含义与组成成分可参考 [JLS15.28](https://docs.oracle.com/javase/specs/jls/se11/html/jls-15.html#jls-15.28)。

因此，常量明显有以下特点：

* final 变量
* 声明时赋值
* 使用常量表达式赋值

然而……这些特点个个都是“坑”，我们一一来看。

首先，常量必然是 final 的，但不必是 static 的。虽然，我们最常见的是静态常量，声明类似 `public static final int i = 42;`，但常量也可以是实例变量，甚至是局部变量。

其次，常量必需声明时赋值。换句话说，空白 final 不是常量。

最后，常量必需用常量表达式赋值。因此，常量必然是基本数据类型或字符串类型。常量表达式组成成分很繁杂，简单概括来说，它是由字面量（即基本数据类型和字符串）或其它常量经过适当操作符运算的表达式。

示例1-1

```java
public class FinalTest {
    private static final int i = 42;    // 静态常量
    private final int j = 43;           // 实例常量

    @Test
    public void test() {
        final int k = 44;               // 局部常量
        final int p;                    // 空白 final，不是常量
        p = 45;
        final Integer q = 99;           // 非基本数据类型和字符串类型变量，不是常量

        System.out.println(i);
        System.out.println(j);
        System.out.println(k);
        System.out.println(p);
        System.out.println(q);
    }
}
```

对于常量，编译器会对常量表达式进行“预先计算”替换为结果字面量，其它对常量的引用也会替换为该结果字面量。

下面的代码就是上例代码编译后再反编译的代码，可以看到这些替换。

```java
public class FinalTest
{
  private static final int i = 42;
  private final int j = 43;
  
  @Test
  public void test()
  {
    int k = 44;
    
    int p = 45;
    Integer q = Integer.valueOf(99);
    System.out.println(42);
    System.out.println(43);
    System.out.println(44);
    System.out.println(p);
    System.out.println(q);
  }
}
```

### 隐式 final

所谓“隐式 final 变量”，是指那些虽然没有 final 修饰，但实际上其缺省修饰符就包括 final 的变量。包括以下几种：

* 接口中的成员变量
* try-with-resources 语句中声明的资源变量
* 多重 catch 子句中的异常参数

接口中的成员变量通常规范要求不用显示 final 修饰，否则认为是“多余”的。

后面两个是 Java 7 加入的新特性，通常规范未作要求，但一个推荐的作法是：在从低版本升级的过渡阶段应该加上 final 以明确语义。

示例：

```java
    interface InterfaceField {
        int implicitlyFinal = 42;       // 隐式 final，接口中的域
    }

    class MyClcass implements InterfaceField {

        public void test() {
            try(InputStream implicitlyFinalResource = new FileInputStream("xxx")) { 
                // 隐式 final，TWR 资源
                System.out.println(implicitlyFinal);
                System.out.println(implicitlyFinalResource);
            } catch (FileNotFoundException | RuntimeException implicitlyFinalException) { 
                // 隐式 final，多重 catch 异常参数
                implicitlyFinalException.printStackTrace();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
```

### 事实 final

局部变量和方法、构造器、lambda、异常参数，如果不会重新赋值，那么就称为“实事（effectively） final”。

注意，这里又提到了异常参数，上文中说多重 catch 子句中的异常参数是隐式 final 的，但是普通的单个 catch 子句中的异常参数不是，它可能是事实 final 的，只要没有重新赋值。

### 写保护的域

System.out、System.err、System.in 都是 static final 修饰的，但通过 System.setOut()、System.setErr()、System.setIn () 可以分别修改它们。这些域被称为“写保护的域”。

《Java 语言规范》中说明这是一个“历史遗留问题”。另一方面看，这些修改方法都是 native 的，因此，我们可以理解为本地方法可以绕过 JVM 限制修改 final 域。

## 方法

final 方法的一个语义是允许编译器根据情况将该方法的所有调用替换为“内联”方式，从而提升效率。

## 综合

### 多线程

多线程情况下，final 域的语义与普通域是不同的。

其中一个明显的区别是，当一个线程中的对象完全初始化时，另一线程保证可以看到该对象的 final 域的值，但不保证能看到普通域的值。

因此，一个可能的安全问题是：即使使用的对象是不可变的，但如果其中的域不是 final 的，那么其它线程就可能看到该域的缺省值。

另外，final 域是可能在对象构造之后修改的，比如反序列化、反射等方法。

通常，这些多少会涉及 Java 内存模型的知识，这里就不一一赘述了。

### 兼容性

通常，一个 final 修饰被去掉前后的代码是兼容的，不会引起错误。

但是，反过来，加上 final 修饰前后的代码通常就不是兼容的。

* 一个类如果加上 final 修饰，那么其子类在运行时会抛出 VerifyError
* 一个方法加上 final 修饰，那么子类重写的方法运行时会抛出 VerifyError
* 一个成员变量加上 final 修饰，那么其它赋值在运行时会抛出 IllegalAccessError

注意，上面讨论的是仅重编译修改部分的情况。在 IDE 中，这种修改往往是引起编译错误。



一个需要特别说明的是——常量。

由于常量会在编译时进行字面量替换，所以修改常量的值在仅重编译修改部分的情况下，该修改将不会被观察到。

## 比较

### final vs. finally vs. finalize()

语义上说，它们几乎没有相关性。

面试中经常把它们放在一起讨论区别，大概仅仅是它们单词的相似性而已吧！但是，你应该清楚它们各自指的是什么。

### final vs. abstract

final 与 abstract 在语义上是互斥的，因此不能同时使用。

### 变量与方法的“重写”行为

子类中可以重新声明与父类中同名的成员变量，即使在父类中已被声明为 final 的。此时，子类中声明的成员变量会隐藏父类中声明的同名成员变量。由于是隐藏，所以子类中的变量和父类中的变量是相互独立的，因此，它们的修饰符可以不同。

与成员变量不同的是，只要方法签名（方法名+参数列表）相同，那就认为是重写，而 final 方法不能被重写。

总而言之，子类中可以重新声明父类中同名 final 成员变量，但是不能声明父类中签名相同的 final 方法。

# 其它

一个与 final 相关的最佳实践是：所有方法参数都应该用 final 修饰。



另一个常讨论的问题是：为什么在内部类中引用的外部变量必须是（事实） final 的？
该问题说法不尽相同，比较可信说法是：内部类使用的外部变量将以值拷贝的形式被内部类中自动生成的隐藏变量持有，因此它们是不同的引用，为了使其同步，因此必须是不可变的。

# 意外？例外

## 常量修改

```java
public static final int i = 42;
public static final String s = null;
```

假设，修改 `i = 43`，`s = "new"`，然后仅重编译修改部分。

那么，其它代码引用 `i` 的地方看到的值还是 42，而 `s` 会看到新值 `new`。

关键在于 null 不是常量表达式，因此 `s` 本身不是一个常量声明，所以修改后能被观察到新值。

## 读取到 final 域缺省值

final 域被初始化之前，存在被读取到其缺省值的可能。

在《Java 解惑》一书中，有如下的示例展示了这种情况：

```java
import java.util.Calendar;

public class Elvis {
	public static final Elvis INSTANCE = new Elvis();
	private final int beltSize;
	private static final int CURRENT_YEAR = 
		Calendar.getInstance().get(Calendar.YEAR);
	
	private Elvis() {
		beltSize = CURRENT_YEAR - 1930;
	}
	
	public int beltSize() {
		return beltSize;
	}
	
	public static void main(String[] args) {
		System.out.println("Elvis wears a size " 
                           + INSTANCE.beltSize() + " belt.");
	}
}
```

以上程序输出：Elvis wears a size -1930 belt.

这个结果很反直觉，要清楚原因，大致需要知道以下几个知识点。

* CURRENT_YEAR 不是常量。因此，不应期望第 10 行代码等效于当前年份与 1930 之差。
* 域初始化是自上而下顺序执行的。INSTANCE 初始化时 CURRENT_YEAR 还没有初始化，仍为缺省值 0。
* 类的循环初始化。Elvis 执行 main() 方法触发了类初始化。首先需要初始化域 INSTANCE，这需要实例化当前类。而构造器中访问了 CURRENT_YEAR，这又要求当前类初始化完成。然而，当前正在执行类初始化，因此这个递归的类初始化请求会被忽略。故取到 CURRENT_YEAR 的缺省值 0。进而在构造器中为 beltSize 赋值为 -1930。

要修正这个错误，需要对静态域初始化器调整排序，使得初始化器出现在任何依赖于它的初始化器之前。即 CURRENT_YEAR 应置于 INSTANCE 之前。

---

# 遗留问题

* 常量为什么必需声明时赋值？
* 常量表达式中怎样算是必要的字符串强制转换？
