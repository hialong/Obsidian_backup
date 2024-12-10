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


除了幻象引用（因为 get 永远返回 null），如果对象还没有被销毁，都可以通过 get 方法获取原有对象。这意味着，利用软引用和弱引用，我们可以将访问到的对象，**重新指向强引用**，也就是人为的改变了对象的可达性状态！

所以，对于软引用、弱引用之类，垃圾收集器可能会存在二次确认的问题，以保证处于弱引用状态的对象，没有改变为强引用

![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20241206001735.png)

关于这些引用的使用可以参考我写的这段代码![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20241206001946.png)


### 动态代理是基于什么原理？
基础知识需要反射的概念

要搞清楚动态代理是什么？
先要知道他解决了什么问题

1. @ 首先他是一个**代理机制**，代理实际上可以看作对调用目标的一个包装，这样我们对目标代码的调用不是直接发生的，而是通过代理完成，这个实际上也可以看作一个装饰器模式的应用
2. ~ 代理还有一个作用，就是解耦，例如 RPC 的调用，框架内部的寻址、序列化、反序列化等，对于调用者往往是没有太大意义的，通过代理，可以提供更加友善的界面，**简而言之**，用 rpc 的描述来说明，就是像写接口一样调用外部方法


简单实现一个代理方法吧![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20241206004610.png)
注意红框的这里，直接生成了一个可以通过不同的参数调用不同的实现类的实例！多么完美！打印结果如下![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20241206004748.png)

以上是他的好处和作用

从 API 设计和实现的角度，这种实现仍然有局限性，**因为它是以接口为中心的**，相当于添加了一种对于被调用者没有太大意义的限制。我们实例化的是 Proxy 对象，而不是真正的被调用类型，这在实践中还是可能带来各种不便和能力退化。

**我们知道 Spring AOP 支持两种模式的动态代理，JDK Proxy 或者 cglib**
如果被调用者没有实现接口，而我们还是希望利用动态代理机制那么我们就可以使用 cglib
如果我们使用 cglib，你会发现，接口的依赖被克服了
cglib 动态代理采取的是创建目标类的子类的方式，因为是子类化，我们可以达到近似使用被调用者本身的效果。

这里贴一下实现代理的方法和创建实例的方法
```java
public class MyMethodInterceptor implements MethodInterceptor {  
    @Override  
    public Object intercept(Object obj, Method method, Object[] args, MethodProxy proxy) throws Throwable {  
        System.out.println("Before method: " + method.getName());  
        Object result = proxy.invokeSuper(obj, args);  
        System.out.println("After method: " + method.getName());  
        return result;  
    }  
}

public class TargetClass {  
    public void someMethod() {  
        System.out.println("Executing someMethod");  
    }  
}

public class Main {  
    public static void main(String[] args) {  
        Enhancer enhancer = new Enhancer();  
        enhancer.setSuperclass(TargetClass.class);  
        enhancer.setCallback(new MyMethodInterceptor());  
  
        TargetClass proxy = (TargetClass) enhancer.create();  
        proxy.someMethod();  
    }  
}

```

通过创建子类完成了动态的代理，不依赖接口

rpc 就是一种动态代理

AOP 通过（动态）代理机制可以让开发者从这些繁琐事项中抽身出来，大幅度提高了代码的抽象程度和复用度。从逻辑上来说，我们在软件设计和实现中的类似代理，如 Facade、Observer 等很多设计目的，都可以通过动态代理优雅地实现。![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20241207001940.png)

### 如何保证集合是线程安全的，ConcurrentHashMap 如何实现高效的线程安全？

Java 提供了不同层面的线程安全支持。在传统集合框架内部，除了 Hashtable 等同步容器，还提供了所谓的同步包装器（Synchronized Wrapper），我们可以调用 Collections 工具类提供的包装方法，来获取一个同步的包装容器（如 Collections.synchronizedMap），但是它们都是利用非常粗粒度的同步方式，在高并发情况下，性能比较低下。

我们知道 Hshtable 是一个线程安全的，简单来说就是将 put get size 等各种方法加上了 synchronized 导致了所有的并发操作都要竞争同一把锁，大大降低了并发操作的效率

hashtable 或者同步包装版本（利用 Collections 工具类的 synchronizedMap 方法）都只是适合在非高度并发的场景下

关于 Collections 的同步包装器要说一下，实际上也就是用了 synchronized 方法作为互斥的 mutex


#### ConcurrentHashMap 是如何实现的？

早期的ConcurrentHashMap 其实现是基于分离锁和 volatile 来实现的
- 分离锁，就是将内部进行分段，Segment 里面存放 HashEntry 的数据，和 HashMap 类似，分段里面的 hash 相同的字段也是用链表形式存放的
	- 分段（Segment）：ConcurrentHashMap 将整个哈希表分成多个段（Segment），每个段是一个独立的哈希表，并且有自己的锁。**这样可以让多个线程同时访问不同的段**，从而减少锁争用，**提高并发性能**
- HashEntry 内部使用 volatile 的 value 字段来保证可见性，也利用了不可变对象的机制以改进利用 Unsafe 提供的底层能力，比如 volatile access，去直接完成部分操作，以最优化性能，毕竟 Unsafe 中的很多操作都是 JVM intrinsic 优化过的
	-  大多数读取操作（如 get）是无锁的，使用 volatile 变量和 CAS（Compare-And-Swap）操作来保证可见性和一致性，关于 volatile 可以看[[多线程相关#Volitale]]

早期实现图如下
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20241207004111.png)

在构造的时候，Segemnt 的数量由所谓的 concurrencyLevel 决定的，默认是 16，如果你用构造函数设置初始容量的话，如果输入是类似 15 这种非幂值，会被自动调整到 16 之类 2 的幂数值。

jdk 8 后的版本 concurrentHashMap