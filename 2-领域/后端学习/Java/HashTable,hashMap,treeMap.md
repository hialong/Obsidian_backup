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

### 有序 ma


