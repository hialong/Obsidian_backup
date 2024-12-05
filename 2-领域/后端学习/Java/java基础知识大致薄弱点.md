---
created: 2024-12-05
updated: 2024-12-05
Type: knowledge
Status: ⌛️ 等待
tags:
---
# 一些感兴趣的面试技术

## 20241205 

### 问题

 1. 谈谈final、finally、 finalize有什么不同
 2.  强引用、软引用、弱引用、幻象引用有什么区别
 3.  动态代理是基于什么原理
 4. 如何保证集合是线程安全的 ConcurrentHashMap如何实现高效地线程安全
 5. 11 Java提供了哪些IO方式？ NIO如何实现多路复用
 6. 12 Java有几种文件拷贝方式？哪一种最高效？
### final finally finalize


final 可以用来修饰类、方法、变量，分别有不同的意义，final 修饰的 class 代表不可以继承扩展，final 的变量是不可以修改的，而 final 的方法也是不可以重写的（override）
>final 并不意味着 immutable 用 final 修饰的 list 仍然可以做添加和删除操作，想要实现 immutable 则需要下面几步
>1. 将 class 自身声明为 final，这样别人就不能扩展来绕过限制了
>2. 将所有成员变量定义为 private 和 final，并且不要实现 setter 方法。
>3. 构造对象的时候，使用深度拷贝来初始化，而不要直接赋值，因为无法保证输入对象不被其他人修改
>4. 如果确实要实现 getter 方法，使用 copy-on-write 原则，创建私有的 copy

finally 则是 Java 保证重点代码一定要被执行的一种机制。我们可以使用 try-finally 或者 try-catch-finally 来进行类似关闭 JDBC 连接、保证 unlock 锁等动作。

finalize 是基础类 java.lang.Object 的一个方法，它的设计目的是保证对象在被垃圾收集前完成特定资源的回收。finalize 机制现在已经不推荐使用，**并且在 JDK 9 开始被标记为 deprecated**。

