---
layout: post
title: Java专题：为什么必须是final的？
categories: Java
tags: Java Java专题
excerpt: 讨论为什么Java内部类访问的外部局部变量必须是final的。
author: "Eric Zong"
---

* content
{:toc}

```java
public class InnerClassAccessFinalVar {
	public static void main(String[] args) {
		int i = 42;
		// i = 100;
		
		class Inner {
			public void test() {
				System.out.println(i);
			}
		}
		
		Inner inner = new Inner();
		inner.test();
	}
}
```

上面的示例代码中，内部类 `Inner` 访问了外部的局部变量 `i` ，因此，变量 `i` 的声明方式是受限的：

> 从内部类引用的本地变量必须是最终变量或实际上的最终变量

Java 8 之前，变量 `i` 必须用 `final` 修饰；Java 8 之后，放宽了限制（语法糖而已），只需变量 `i` 实际上是 `final` 的（所谓“实际上”是说虽然没有 `final` 修饰，但是并没有重新赋值）。

# 探索

这个限制看起来很奇怪，为什么要有这样的限制？我一直很好奇。

## 一个不令人信服的解释

第一个解答我记得好像是李刚的《疯狂Java讲义》（下称“《讲义》”），是这么解释的：

> 对于普通局部变量，作用域在方法内，但内部类可能产生隐式“闭包（Colsure）”，使局部变量脱离方法继续存在，即扩大作用域。

当我看到这个解释时是困惑的，“作用域扩大”的说法本身就很矛盾，根据 [wiki](https://zh.wikipedia.org/wiki/%E4%BD%9C%E7%94%A8%E5%9F%9F) 对作用域的解释来看，Java 采用的是静态作用域：

> **静态作用域**又叫做词法作用域，采用词法作用域的变量叫**词法变量**。词法变量有一个在编译时静态确定的作用域。词法变量的作用域可以是一个函数或一段代码，该变量在这段代码区域内可见（visibility）；在这段区域以外该变量不可见（或无法访问）。词法作用域里，取变量的值时，会检查函数定义时的文本环境，捕捉函数定义时对该变量的绑定。

因此，从概念上来说，局部变量就不可能脱离方法而存在。事实上，我怀疑《讲义》指涉的不是空间上的代码区域，而是代码“异步执行”的时间区域。比如：

```java
public class InnerClassAccessFinalVar {
	public static void main(String[] args) {
		int i = 42;
		
		Runnable runner = new Runnable() {
			public void run() {
				try {
					Thread.sleep(1000);
					System.out.println(i);
				} catch (InterruptedException e) {
					e.printStackTrace();
				}
			}
		};
		
		Thread thread = new Thread(runner);
		thread.start();
	}
}
```

这跟 stackoverflow 上 [Cannot refer to a non-final variable inside an inner class defined in a different method](https://stackoverflow.com/questions/1299837/cannot-refer-to-a-non-final-variable-inside-an-inner-class-defined-in-a-differen) 这个问题的一个回答类似，而这个回答被认为是错误的。

## 进一步深入

为了弄清楚到底是为什么，又看了许多相关博客、知乎及 stackoverflow 的问答，找到了一些比较可信的解答。

内部类中使用外部局部变量，涉及到变量捕获的问题。

简而言之，Java 只做“值捕获（capture-by-value）”，即在内部类中为外部局部变量创建副本（这跟方法参数的“值传递”类似）。在内部类中使用的“所谓外部局部变量”仅仅是“真实外部局部变量”的一个拷贝，因此，在内部类中为变量重新赋值不会影响外部变量。

重新赋值导致的问题是：内外两个变量将不再同步。

为了同步，所以要求该变量必须有 `final` 修饰，使其不可变。

## 个人思考

关于变量捕获那部分知识我倒是没有什么质疑，但这个逻辑推理过程我表示有点牵强了，不理解。

感觉上这种不同步问题是代码可控的，因此，规范说明即可，提升到语法限制的程度是否是过于严格了呢？

# 小结

最后简单梳理下整个逻辑。

Java 只实现了“值捕获”将外部局部变量拷贝到内部类中，因此，内外是两个独立的引用，重新赋值将导致不同步的问题。因此，为了避免内外变量不同步，变量需声明为不可变。

# 参考

* [JVM的规范中允许编程语言语义中创建闭包(closure)吗？](https://www.zhihu.com/question/27416568/answer/36565794)

* [Cannot refer to a non-final variable inside an inner class defined in a different method](https://stackoverflow.com/questions/1299837/cannot-refer-to-a-non-final-variable-inside-an-inner-class-defined-in-a-differen)

* [为什么必须是final的呢？](http://cuipengfei.me/blog/2013/06/22/why-does-it-have-to-be-final/)

