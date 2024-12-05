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

finally 则是 Java 保证重点代码一定要被执行的一种机制。我们可以使用 try-finally 或者 try-catch-finally 来进行类似关闭 JDBC 连接、保证 unlock 锁等动作。