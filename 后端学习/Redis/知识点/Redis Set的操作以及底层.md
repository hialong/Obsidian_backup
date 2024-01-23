---
Created: 2024-01-17
Updated: 2024-01-17
Type: knowledge
Status: 🎃已完成
截止日期: 
目标: 
领域: 
tags:
  - Redis操作
  - RedisSet
---


## redis Set

- 是什么 

  无序的集合

 - 场景 

   去重场景

 - 怎么用

    -  创建
        -  SADD 
    -  查询
        -  SISMEMBR : 查询是否为成员
        -  SMEMBRS : 返回集合中所有成员的列表 
        -  SCCARD： 返回集合中成员个数
        -   SSCAN : 遍历，后面跟下标和count，还可以用match进行模糊查询
        -  SINTER : 以前面的集合判断交集
        -  SUNION：并集
        -  SDIFF：different 多个set的去重，以前面的为基准
    -  更新
        -  SADD
        -  SREM：删除
    -  删除
        -  DEL

###  底层实现

Set的编码方式：
	1. intSet
	2. HashTable ^c089ec

![image-20231219012529951](D:\\study\img\image-20231219012529951.png)

- hashTable 的特点，就是能在O(1)的复杂度下查找到需要的数据
- intset更节约内存 ^20a417
	- 如果集群元素都是整数，且元素数量不超过512个，就采用inset编码，

intSet 结构如下图，结构紧凑，内存占用少，==但是需要查询的时候，要二分查询==, 也就是时间复杂度是 O（logN）
	![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240105010432.png)

#RedisHashTable [[Redis hashTable 底层以及操作]]

如果不满足intSet的条件，就要用HashTable，结构如下图，hashTable查询元素性能很高，O(1)时间就能查到一个数据是否存在
	![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240105010603.png) ^35d9bd
