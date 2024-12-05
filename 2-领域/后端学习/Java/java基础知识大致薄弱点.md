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

==final==
final 可以用来修饰类、方法、变量，分别有不同的意义，final 修饰的 class 代表不可以继承扩展，final 的变量是不可以修改的，而 final 的方法也是不可以重写的（override）
>final 并不意味着 immutable 用 final 修饰的 list 仍然可以做添加和删除操作，想要实现 immutable 则需要下面几步
>1. 将 class 自身声明为 final，这样别人就不能扩展来绕过限制了
>2. 将所有成员变量定义为 private 和 final，并且不要实现 setter 方法。
>3. 构造对象的时候，使用深度拷贝来初始化，而不要直接赋值，因为无法保证输入对象不被其他人修改
>4. 如果确实要实现 getter 方法，使用 copy-on-write 原则，创建私有的 copy,也就是 new 一个新的返回，不要返回原始的变量

==finally==
finally 则是 Java 保证重点代码一定要被执行的一种机制。我们可以使用 try-finally 或者 try-catch-finally 来进行类似关闭 JDBC 连接、保证 unlock 锁等动作。当然，特殊情况就是说他可能会在系统结束时候不生效

finalize 是基础类 java.lang.Object 的一个方法，它的设计目的是保证对象在被垃圾收集前完成特定资源的回收。finalize 机制现在已经不推荐使用，**并且在 JDK 9 开始被标记为 deprecated**。
>finalize 会吞掉 throwable 的异常，可以使用 java.lang.Cleaner 来替换掉原有的 finalize 实现


###  强引用、软引用、弱引用、幻象引用

这里主要强调几点，幻象引用也叫虚引用
强引用就是垃圾回收不会碰的引用
软引用是只有 jvm 认为内存不足的时候才会试图引用
其余两个都会直接被垃圾回收
这里贴上关系图
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20241206000106.png)


除了幻象引用（因为 get 永远返回 null），如果对象还没有被销毁，都可以通过 get 方法获取原有对象。这意味着，利用软引用和弱引用，我们可以将访问到的对象，重新指向强引用，也就是人为的改变了对象的可达性状态！

所以，对于软引用、弱引用之类，垃圾收集器可能会存在二次确认的问题，以保证处于弱引用状态的对象，没有改变为强引用

另外强引用我觉得是有一定的使用场景的，比如有一个对象，本身没有强引用，