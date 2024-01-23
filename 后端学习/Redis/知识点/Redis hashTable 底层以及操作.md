---
Created: 2024-01-17
Updated: 2024-01-22
Type: knowledge
Status: 🎃已完成
截止日期: 
目标: 
领域: 
tags:
  - Redis操作
  - RedisHashTable
---


## hashTable

-  是什么

  - hash表，特点是O(1)的查找效率

- 数据结构

  - ![image-20231219234355756](D:\\study\img\image-20231219234355756.png)

  - 总的结构是dictht，里面有table、size、sizemask、used

    - table里面指向我们的dictEntry，里面是保存的信息

    - size是表示数组的大小

    - sizemask(也叫hash掩码)为size-1，使用于插入数据的时候，用与操作和插入值的hash值判断插入数据的位置
	    - 插入数据的时候，先是通过hash函数计算出key的哈希值，然后与hash掩码做运算得到索引值，索引值就是这个数据在HashTable中的储存位置 ^1840ae

    - used表示的是使用了几个，比如上图只有0 2 有数据，1对应的值为null，所以used 为2

    - 1 2后面是链表，出现hash冲突的时候会有链表结构

```C
// from Redis 5.0.52 
typedef struct dictht {
  dictEntry **table;
  unsigned long size;
  unsigned long sizemark;
  //键值对数量
  unsigned long used;
 } dictht;
```

### 渐进式扩容

- 渐进式扩容
	-   - 新表h1和旧表h0
	  - 渐进式扩容 
	  - 从h0->h1扩容
	  - 负载因子-扩容和缩容
	  - h1的空间是原表used的2倍的2次方幂
	-hashTable的扩容方式比较特殊，为了实现渐进式扩容，Redis中实际没有之间把==dicthth==暴露给上层，而是又做了一层封装 ^c709f0
	  - 封装层如下图 dict
		  - ![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240106183838.png)
		里面包含了两个hashTable结构，分别是 "ht[0]" 和 "ht[1]",这个指向的才是真正的表结构==dictht==
	  - 如何触发扩容？
		  - 每次往字典里面添加元素的时候，都会检查一下负载因子，一旦需要扩容就会触发渐进式扩容，这里提到了新的知识点，[[#^5c3450|扩容时机]]
		   
	  - 渐进式扩容流程（大致流程）
		 1. 首先式为新的HashTable1 分配空间，新表的大小取原表的两倍大小向上取2的N次方幂），比如原来的表大小是500，那么新表就是取1000的向上取整的最接近的2次方幂1024
		 2. 然后就是将封装的dict里面的rehashidx从静默状态（-1）变成0，这就代表着rehash工作正式开始
		 3. 然后就是把旧数据迁移到新表，而且同时，每次又触发了增删改查的操作的时候，程序也会顺带迁移表0上的数据，随着迁移的进行，最终，数据会全部迁移到表【1】上，这个时候，就换名字，将ht[1]和ht[0]指针对象互换，同时把偏移索引l的值设为-1，表示Rehash操作已完成。这个事情也是在Rehash函数做的，每次迁移完一个元素，会检查下是否完成了整个迁移：
	![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240106190503.png)
			 ^5f424e


### 负载因子
- 负载因子
	- - Redis提出了一个负载因子的概念，负载因子表示目前Redis HASHTABLE的负载情况，是游刃有余，还是不堪重负了。我们设负载因子为k，那么k=ht[0].used/ht[0].size，也就是使用空间和总空间大小的比例。Redis会根据负载因子的情况来扩容：
			  1. 负载因子大于等于1，说明此时空间已经非常紧张。新数据是在链表上叠加的，越来越多的数据其实无法在0(1)时间复杂度找到，还需要遍历一次链表，如果此时服务器没有执行BGSAVE或BGREWRITEAOF这两个命令，就会发生扩容。复制命令对Redis的影响我们后面在原理篇再讲。 ^5c3450
			  2. 负载因子大于5，这时候说明HASHTABLE真的已经不堪重负了，此时即使是有复制命令在进行，也要进行Rehash扩容。