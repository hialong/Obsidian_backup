#Redis操作 #RedisList

## redis List

- 是什么：list
- 怎么用：
  - 创建 lpush rpush（分别是左边开始往里push数据，和右边开始往里push数据）
  - 查询 llen（查有多少个） lrange（lrange key start stop 查看start到stop为角标的元素，如果是负数，就从后面开始查）
  - 更新 lpush rpush lpop（从左边开始pop） rpop（同理） lrem（从左边开始remove）
  - 删除 DEl UNLINK

![image-20231217222008380](D:\\study\img\image-20231217222008380.png)

### list底层实现

list对象编码：

1. ziplist ^5f2ba2
	1.  要满足条件采用ziplist编码：
		1. 列表对象保存的所有字符串长度都小于64字节
		2. 列表对象元素个数少于512个

3. linkedlist

4. quickList(综合了上面的两个实现，双向链表，内部是ziplist)目前5.0.5全是由quickList实现

   

   linkedlist 表头定义了len，所以查询长度的时间复杂度是0（1）

4. list由  结构头（header）{里面有结构 zllen记录了节点的数量，但是如果节点大于65535,那就需要遍历了，因为zllen是2字节的，只能存这么多}   和 数据字段组成

## redis压缩列表（ziplist）

^2bb21b

	- 是什么：内存紧凑的列表，适合小数据量
	- 场景：list set，小数据量
	- 优点：内存占用小
	- 缺点是，更新的时候，里面的所有数据都要移动，性能成本高

外面是表头，数据指针指向的就是ziplist实际的部分了
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240105002312.png)


![image-20231217232526448](D:\\study\img\image-20231217232526448.png)

zlend表示list的终结 二进制的8个1组成，代表255

zllen 为两个字节长度，一般查数据的的时候，zllen里面可以存 2^16次方减一位数，超过了就要遍历，==所以一般情况下，查询节点个数的时间复杂度为O(1),超过一定阈值，则查询节点的复杂度就变成O(n)== ^1f1cf4

entry表示的是实际的数据，

> entry 里面结构： prevlen encoding entry-data,分别表示，记录的前面entry的长度、编码、实际数据

-  连锁更新

  > 连锁更新就涉及到上面这个prevlen了，这里记录了上一个节点的数据长度，如果前一个节点长度小于254字节，那么prev就用1字节长的空间保存这个长度，否则就用5字节的空间保存这个长度
  >
  > 这里就可能触发连锁更新，即prevlen更新长度以后，使得本节点长度上涨了4位，再使得后面的节点再更新prevlen

![image-20231219010107838](D:\\study\img\image-20231219010107838.png)

- 怎么解决连锁更新

  - listpack

    结构变成了 

![image-20231219010259819](D:\\study\img\image-20231219010259819.png)

encoding-type是编码类型

element-data是数据内容

element-tot-len存储整个节点除它自身之外的长度


zipList的更新
	 由于ziplist底层是偏向数组的结构，里面的entry为了节约内存都是相邻的，而且整个ziplist的内存是统一分配的，所以插入数据的时候，主要有两件事
		 1. 为整个ziplist重新分配空间，主要是为了给新节点分配空间（这一步如果ziplist刚好没有多余空间，那么我们就要把ziplist整个迁移到别的地方去）
		 2. 插入节点,插入的数据后面的节点都要往后移动，腾出空间给新节点
	
	 所以，ziplist在数据较多的时候（没达到转换成linkedList的标准前），会复制较多的空间，导致很多内存复制 ^228531
## Linked List

> 如果不满足zipList的条件 [[#^5f2ba2]]，则使用linkedList 的编码

格式如下

![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240105002807.png)


而且，linkedList里面的表头定义了链表包含节点数量的字段 len
	结构如下
	![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240105005306.png)
	所以在linkedList的编码下，查询节点个数的复杂度是O(1) ^c4c8df


## quickList横空出世

上面两个list都各有缺点，ziplist 在数据插入稍多的时候插入数据会导致很多的内存复制（原因）[[#^228531]] ，linkedList表稍微大一点，节点数量就很多，会导致不少的内存占用，所以，在 ==3.2==版本以后，就引入了quickList，==他是集二者优点之数据结构==

他的数据结构如下
	![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240105004503.png)

以linkedList的结构，节点里面储存的是ziplist，当数据量比较少的时候，他只有一个节点，相当于一个ziplist，数据很多的时候则是同时拥有了ziplist和linkedlist的优点
	1. 数据上节约内存
	2. 更新提高更新效率

