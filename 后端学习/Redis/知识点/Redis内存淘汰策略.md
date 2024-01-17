---
Created: 2024-01-17
Updated: 2024-01-17
Type: knowledge
Status: 🌱 完成
截止日期: 
目标: 
领域: 
Tags:
---

## 内存淘汰（内存满了怎么办）

### 八个淘汰策略

​	![image-20231222075727013](D:\\study\img\image-20231222075727013.png)

不开启淘汰数据就是拒绝新的数据



#### 部分介绍

##### LRU

> 淘汰最近最久未使用的数据，latest Recently Used

成本：双链表，巨大的内存损耗

redis选择近似LRU算法

![image-20231222080207919](D:\\study\img\image-20231222080207919.png)

LRU的不足

 他是用最近访问时间去淘汰内存的而忽略的频率，有时候会出现经常访问的一个数据和偶尔才被访问的数据在一起，反而经常访问的数据被淘汰了,所以有新的淘汰算法  LFU

![image-20231222080456083](D:\\study\img\image-20231222080456083.png)

![image-20231222080551943](D:\\study\img\image-20231222080551943.png)

##### LFU

![image-20231222080735687](D:\\study\img\image-20231222080735687.png)

在淘汰时候，他会去检测一下上次的访问和最近访问的时间的衰减再去淘汰





##### 注

内存回收什么时候发起

​	每次读写的时候，都会调用ProcessCommand函数，processCommand又会调用freememoryIfNeed，满了就会尝试释放内存

## Redis内存结构

![image-20231222081538376](D:\\study\img\image-20231222081538376.png)





dict(字典中存储数据)

![image-20231222081708058](D:\\study\img\image-20231222081708058.png)

  过期时间戳

![image-20231222081807472](D:\\study\img\image-20231222081807472.png)
