---
created: 2024-12-11
updated: 2024-12-11
Type: knowledge
Status: ⌛️ 等待
tags:
---
## 他们有什么不同和相同点？

首先他们都是一些最常见的 **Map 实现**，是以键值对的形式存储和操作数据的容器类型

hashTable 是早期一个实现，本身是同步的，性能差，不支持 null 键值对

hashMap 是广泛的哈希表实现，行为上大致与 HashTale 一致，主要区别在于 HashMap
	1. 非同步的操作
	2. 支持 null 的键值

TreeMap 则是基于红黑树的一个实现顺序访问的 Map，他的 put get remove 之类的操作都是 logn 的复杂度，具体顺序可以用 Comparato指定


## 深入

### 有序 map
有序的 map 包括 linkedHashMap 和 TreeMap
首先是 LinkedHashMap 


**linkedHashMap** 通常提供的是一个遍历符合插入顺序，他的实现是通过为条目（键值对）维护一个双向链表。通过特定构造函数，我们可以创建反映访问顺序的实例，所谓的 put、get、compute 等，都算作“访问”。

这种行为适用于一些特定应用场景
例如，我们构建一个空间占用敏感的资源池，希望可以自动将最不常被访问的对象释放掉，这就可以利用 LinkedHashMap 提供的机制来实现


例如这样![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20241211003822.png)

而对于 treeMap 来说，他的整体顺序是由键的顺序关系决定的，通过 Comparator 或者 Comparable 来决定的


### HashMap 的源码分析

![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20241211004102.png)

大致的内部结构如上图所示，实际上的数据结构被分成了一个个桶

从源码来看，hashMap 的初始化似乎只是设置了一些初始值？![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20241211004509.png)
所以有理由怀疑，实际的初始化会不会是在调用 put 方法的时候才初始化的？依据 lazy-load 原则，在首次使用时被初始化，似乎很有可能，那么就看看 put 的源码吧
如下
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20241211004651.png)
那么很有可能就在 putVal之中
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20241211010153.png)

可以看到在 put 的过程中，
- 如果 tab 为 null，resize 方法会负责初始化它
进入 resize 方法，可以了解到 resize 方法就是负责扩容和初始化的![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20241211010906.png)
至于扩容的部分可以看这里![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20241211011430.png)
- 门限值 threadhold等于（负载因子）x（容量），如果构建 HashMap 的时候没有指定它们，那么就是依据相应的默认常量值。
- 门限通常是以倍数进行调整 （newThr = oldThr << 1），我前面提到，根据 putVal 中的逻辑，当元素个数超过门限大小时，则调整 Map 大小。
- 扩容后，需要将老的数组中的元素重新放置到新的数组，这是扩容的一个主要开销来源![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20241211011622.png)
另外可以看到超过门限值就扩容![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20241211011812.png)


### haspMap 中链表结构（hash 碰撞）也叫 bin 的树化

![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20241211012040.png)
当链表过长的时候就会树化
