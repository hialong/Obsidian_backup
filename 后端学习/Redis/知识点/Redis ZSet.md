---
Created: 2024-01-17
Updated: 2024-01-17
Type: knowledge
Status: 🌱 完成
截止日期: 
目标: 
领域: 
tags:
  - Redis操作
  - RedisZSet
---
## Redis ZSET

- 是什么

  有序集合，也叫SortedSet ,存储的分数是抽象的分数，任何指标都能抽象成分数

- 场景

  排行榜
 
- 怎么用
  
- ![image-20231220232953541](D:\\study\img\image-20231220232953541.png)

简单看一些增删改查的场景
	![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240106211735.png)
	![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240106212228.png)
	![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240106212304.png)





- 底层结构 ^c884be
  - 一个是ziplist  [[Redis List底层以及操作#^2bb21b|ziplist]]
  - 一个是skiplist（跳表） [[Redis 跳表]] + hashTable

![image-20231221002451815](D:\\study\img\image-20231221002451815.png)

`只有上面的两个条件同时满足才会用ziplist`


而第二种结构如下(跳表和HashTable配合使用)
	为什么配合使用？
		#RedisHashTable #Redis跳表 
		当Zset要根据成员来找分数的时候，使用字典实现，时间复杂度就是O(1)，当Zset执行范围操作的时候，比如`ZRANK`、`ZRANGE`，就用原本有序的跳表来实现[[Redis 跳表]]
	
![image-20231221002636300](D:\\study\img\image-20231221002636300.png) ^b8687e

上面的跳表提供了跳表范围查询等能力，hashtable则是提供了O(1)复杂度的查询分数的能力