---
title: scala类中成员的访问修饰符
date: 2016-03-12 12:39:55
tags: scala
categories: 技术
---

scala中类的成员（不考虑类，只考虑类中的成员：属性和方法）一共有三种访问修饰符：public（默认）、private和protected。
java有四种访问修改符：public、private、protected和package-private（默认）。
其中，public是scala默认的访问修饰符，package-private是java的默认访问修饰符。
scala没有package-private（包内可见）修饰符，但是可以给private和protected加包和对象权限。
java的protected修饰符为包内和子类可见，scala仅为子类可见，更为严格。

***scala属性：***

| 访问修饰符                        | 最高可见范围                        |
| ---------------------------------|:----------------------------------:|
| public                           | 全体可见                            |
| private                          | 本类及伴生对象内可见                 |
| private[包名(e.g.,ustc.marin)]   | 本类及伴生对象及ustc.marin包内可见    |
| private[对象引用名(e.g.,this)]    | this对象可见（this.f行，that.x不行） |
| protected                        | 子类可见                            |
| protected[包名(e.g.,ustc.marin)] | 子类及ustc.marin包内可见             |
| protected[对象引用名(e.g.,this)]  | 同private[this]                    |

可见，加了包或对象权限的private和protected，可能扩大权限，也可以缩小权限。

scala运行在jvm上，它的编译结果也是class文件，用jd-gui反编译成java文件，可以看到：
public的属性会编译成private的属性和public的getter setter方法；
private的属性会编译成private的属性和private的getter setter方法；
private[this]的属性会编译成private的属性，没有方法；
private[包]、protected、protected[包]、protected[this]会编译成private的属性，public的方法 ——这个很奇怪，感觉对不上，protected的为什么会反编译成public的呢？这不乱了吗？其实不乱，scala是静态语言，它的运行时把.scala编译成.class文件在jvm上运行，把.scala编译成.class这个过程会做很多事情，这其中就包括检查访问权限，如果不符合scala标准，直接报错，到不了jvm运行。所以这里建议： **不要想反编译成java文件是什么样的，记住scala的规则就好了。**


***方法（函数也一样）：***

- *方法体：* 总体上和属性一样。

- *方法参数：* 方法的参数必须都是public(默认，不用写，写会报错) val的，没的挑。
a: Int, b: String = public val a: Int, public val b: String。


***类参数：***
- *带变量修饰符（val/var a: Int（带变量修饰符，一定带访问修饰符，因为public默认）、private val/var a: Int ...）：*  和属性一样，生成私有字段，公有或私有getter setter方法。
- *不带变量修改符（类似方法参数的写法：a: Int, b: String ）：* 如果有其它方法（不包括构造方法。scala中直接写在类里，和属性方法并列的语句，会编译到构造方法里）使用了类参数字段，则生成私有字段；如果没有被使用，则连字段都不生成，getter setter方法更没有了。因为这种情况肯定不会生成getter setter方法。所以**要想在其它类里访问不带访问修改符的类参数，得在被访问的类里另外定义属性=类参数或写getter setter方法**。
