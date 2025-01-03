---
Created: 2024-01-17
Updated: 2024-01-17
Type: knowledge
Status: 🌱 活跃
截止日期: 
目标: 
领域: 
Tags:
---
# Redis学习

- 推荐5.05
- 先对象
- 再执行流程
- 持久化
- 场景
- 集群
- 自测题
#Redis操作 
#### 面试题

 - Redis为什么需要持久化
   	- 持久化是说把数据存到磁盘中，redis虽然大多数时候是缓存场景，但是也不想重启以后缓存都不存在了，如果缓存一下消失了，那么数据就一下打到内存里面了，而且，Redis本身也可以做存储，这就更需要持久化了
- RDB和AOF的本质是什么
  - 一个是存储二进制文件，一个是记录命令的日志


# 面试题部分（感觉比较不懂的题目记录一些）

## 对象

### 	1. 浮点型在String是用什么表示的


> 首先String的底层编码有三种
>
> 1. int （记录数字的）
>
> 2. embStr 
>
>    字符串长度比较小的时候用这种，这种格式的优点在于，字节头和内容是在一起的，内容紧凑，占空间小，但是缺点是，他是被设置成只读的，一旦有任何改动，就会变成raw格式
>
> 3. raw
>
>    超过一定阈值就会变成raw的格式，5.5的话是44，占空间
>
> 那么可知，浮点型的数据不是int，所以，会用raw或者embstr来表示，具体用哪个编码要看字符串长度

### 2. String可以有多大（512mb）

### 3.redis字符串是怎么实现的

​	首先，redis的底层字符串的实现有三种

1. int
2. raw
3. embstr

int就是存储一个整型，就可以用一个long去存储这个数据，int超过long的话 ，就看阈值，会变成embstr或者raw的格式，目前较新的版本是44（5.0.5）

embStr是两部分放在一起同时分配内存，而raw则是分开分配内存，所以embstr占用内存更小

embstr他的优点是紧凑，但是他被设置成只读的样式，一旦发生改变，数据格式就会改成raw的格式

raw的话，就是两部分内存分开，方便修改数据

而embstr和raw则是两部分结构，redisObj和sdsHdr这俩部分，redisObj就是用来说明数据是不是string、解码方式是raw还是embstr的，而sdshdr就是封装的数据层，提供了原生c语言达不到的能力

其中sdshdr的部分就是封装了一层，里面直接存储了len（长度）、alloc（就是分配的空间）、以及数据，这样他查询字符串长度不需要遍历，直接O(1)的复杂度，而且判断字符串也不用用 /0去判断了，更安全，然后他还有分配了冗余空间，当字符串小于1M的时候，冗余空间就是字符串的大小，当字符串大于1M的时候，冗余空间就是1M

### 4. 为什么embStr阈值是44

redis采用的jemalloc作为内存分配器的，然后因为redisObj和sdshdr里面的一些结构还有结尾的\0符号占据了一些字节，本身这个分配器是以64个字节为单位分配数据的，然后占据的部分刚好有64-44=20，所以44长度加上刚好一个内存单元

### 5. sds有什么用

原本呢，redis是用C语言写的，C语言里面的字符串是用/0结尾的char数组来表示的，但是这样的话，会有很多问题，包括安全、更改长度以及什么修改字符串啥之类的问题，所以redis封装了一下，

sds里面自带了len，长度查询就是O（1）的复杂度，而且判断字符串结尾也不需要用/0来判断了

然后里面分配了冗余内存，方便修改，规则就是 min（len 1M）



### 6.List是完全先入先出吗

list是双端操作对象，头尾都能pop和push

### 7.list的底层编码是什么

底层编码有两种，一种是ziplist、一种是linkedlist

ziplist结构更紧凑，占用空间小，数据直接是直接放在一起的，优点是占内存小，缺点是数据比较多的时候，修改会复制很多的空间

linkedList结构更松散一些，数据之间是链表的形式，优点是修改方便，缺点是本身占用内存比较大



所以在3.7版本以后，出现了quickList，综合了两者的优点，所有的list全部由quickList实现

​	他是以链表形式存储ziplist，数据少的情况他就是一个ziplist，数据多的话，由会变成linkeslist的结构，里面存储的是ziplist而不是单个数据了



### 8. ziplist是怎么压缩数据的

ziplist是一个双端的链表，而且是占据一块完整的空间，一般的双端链表就是数据之间的位置差距比较大，但是ziplist会把数据放在连续的内存里面成为一个整体

### 9. 在ZIPLIST数据结构下，查询节点个数的时间复杂度是多少?

ziplist在头部定义了一个zllen用于记录数据，是两个字节的大小，就是超过了两个字节2^16-1的大小，那么就会失效，需要遍历,复制度就是O（n）

> 一个字节多大？
> 	一个字节等于8bit,为16位，所以数字最大就是2的16次方-1位，65535

[[Redis List底层以及操作#^1f1cf4]]

### 10. LinkedList编码下，查询节点时间复杂度多少？

LInkedList里面的时间复杂度也是O（1），他的表头里面就包含了节点的数量
[[Redis List底层以及操作#^c4c8df|linkedList的查询时间复杂度]]

### Set的编码方式

采用了整数和字典的方式，其中，如果集群数据都是整数，且整数数量不超过512个，那么就会采用intSet的方式，否则就会采用hashTable的方式编码，hashTable编码查询效率很高，能在O(1)的时间内找出一个数据是否存在
[[Redis Set的操作以及底层#^c089ec]]

### Set是有序的吗
从Set的底层编码来看，Set的底层有两种，一个是IntSet,一个是HashTable，前者是有序的，后者是无序的，但是整体来看应该视为无序的
[[Redis Set的操作以及底层#^c089ec]]

### Hash的编码方式是什么
Hash的底层有两种编码方式，一个是zipList，这里是把hash的field当作entry存到ziplist里面，一种是hashtable，在元素少的时候用zipList，元素多的时候用hashTable
[[Redis Hash#^ebb717]]

### Hash查找某个key的平均时间复杂度是多少？
他有两种编码方式，查找key的复杂度也不同，如果是ziplist的编码方式，查找一个key的复杂度就是O（n）因为需要遍历[[Redis Hash#^3c724e]]
如果是hashTable 的编码方式，那查询的复杂度就是o(1)

### Redis中HashTable查找元素总数的平均时间复杂度是多少?
hashTable的表头里面包含了储存键值对数量的字段，叫used ，所以复杂度为o(1)
#RedisHashTable 
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

### Set为甚要用两种编码方式

首先Set底层有两种编码方式[[Redis Set的操作以及底层#^c089ec|Set的两种编码方式]]
intset的编码方式在较小的时候，更节约内存，
而HashTable 的编码方式查询更快，更适合大数据的场景

### 一个数据在hashTable中的存储位置，是怎么计算的
[[Redis hashTable 底层以及操作#^1840ae|索引值]]
通过计算key的哈希值与哈希掩码一起计算出索引值，索引值就是其储存位置

### hashTable怎么扩容
[[Redis hashTable 底层以及操作#^5f424e|渐进式扩容]]
渐进式扩容，redis首先并没有直接把hashtable的结构暴露出去，而是做了封装，封装里面存了两张表和一个rehashIdx的标记，一张表是数据，一张表是预备扩容的空表
	首先程序会为HashTable的1号表分配空间，新表大小为第一个大于等于原表used的2次方幂。在rehash进行期间，标记位rehashidx从o开始，每次对字典的键值对执行增删改查操作后，都会将rehashidx位置的数据迁移到1号表，然后将rehashidx加1，随着字典操作的不断执行，最终0号表的所有键值对都会被rehash到1号表上。之后，1号表会被设置成0号表，接着在1号表的位置创建一个新的空白表。

### Zset有几种编码方式
[[Redis ZSet#^c884be|Zset的编码]]
有两种，一种就是ziplist，一种就是跳表加字典

### 跳表编码模式下，查询节点总数的平均复杂度是多少？
[[Redis 跳表#^b71a01|查询节点总数复杂度]]

### 为什么跳表和 HashTable要配合使用？（zset）

[[Redis ZSet#^b8687e]]
### 跳表插入一条数据平均复杂度是多少

[[Redis 跳表#^39e5b0]]

## Redis执行
#Redis单线程
### Redis是单线程还是多线程

核心逻辑一直都是单线程的，一些异步删除是多线程的，6.0以后就网络I/o都是多线程的了

### Redis为什么选择单线程做核心处理
[[Redis单线程，还是多线程？#^f15b30]]

### Redis单线程性能如何？
[[[Redis单线程，还是多线程？#^d66aec]]]
另外还有6.0后引用了多线程，而且默认关闭，这些也可以扩展回答看看

## Redis持久化

### RDB和AOF的本质区别是什么
[[Redis持久化#^2a63d7]]
本质就是一个是快照，一个是日志追加

### RDB持久化的触发时机
[[Redis持久化#^1036f5|如何触发RDB]]


### AOF刷盘的触发时机
首先是AOF的流程，实际上分为请求到来，请求处理，然后就是写入AOF文件，而在写入的流程里面，我们就有了刷盘的动作
时机时机上是看我们配置的策略  [[Redis持久化#^013abe|AOF写入流程]]

### RDB对主流程有什么影响
[[Redis持久化#^83f61a]]


### AOF对主流程有什么影响？
[[Redis持久化#^f91732]]

### AOF混合持久化方案是什么？
[[Redis持久化#^125e38|混合持久化]]


### 简单描述AOF重写流程
[[Redis持久化#^0a9ef9]]

## Redis场景

### Redis 缓存是如何应用的？或者说你们用的是什么样的缓存方式

一般来说用到的多的是Cache Aside 也就是旁路缓存[[Redis缓存基础#总结]]

### Redis设置缓存用什么命令？

Set 命令，详见 [[Redis缓存基础#^7205e7|怎么设置缓存]]

### 旁路缓存模式下的查询流程讲一讲？
查询的话，先去查缓存，然后缓存里没有就去查数据库，数据库没有就返回结果，有的话，就先更新缓存再返回结果

### Redis 做旁路缓存，如果 Mysql 更新了，这时候应该怎么去做他的缓存？
正常情况下，我们就是采用的就是新增的时候，就是正常的写入数据库然后加到缓存里，如果是更新数据，就去删除缓存中对应的 key value，如果删除失败也不会影响核心流程，我们直接忽略就行，因为我们还做了过期时间来兜底

## 分布式锁

### 分布式锁实现的要点是什么？

先说怎么做，再讲解，不然显得像背书的
我们就是比较重的方式去做的，用 redis 的 `set key value nx ex second` 配合多机部署做的，value 里面存的是时间戳凑出来的 id，这样每个线程只能解锁自己的，然后解锁是用的 delete，直接删除锁，使用了 Lua 脚本保证了原子性
1. 首先分布式锁我们实现需要有四大特性，互斥安全对称可靠
2. 互斥性很简单，set 一个 key 值作为锁，利用 redis 里面的 set key value nx 如果 key 有值会返回 0 的特点，我们可以轻松做到互斥
3. 安全性是说，锁需要及时解锁，不能因为一些特殊情况导致卡住，设置过期时间，保证不会卡太久
4. 可靠性就是集群和多机部署
[[分布式锁（很重要）]]

### 你提到了Lua，Lua 一定能保证原子性？
lua 脚本本身不具备原子性，但是 Redis 是单线程执行的，所以 lua 打包在一起的花会当作一个流程来执行的，整个流程会保证原子性，（这个流程是指，查询锁是不是自己的，然后如果是自己的就删除）

### 分布式锁是完全可靠的嘛
没有完全可靠的分布式锁，关键的业务还是要靠幂等来兜底，（幂等性指进行了多次操作，得到的结果是一致的）

### RedLock 是什么？

RedLock 就是集群化部署的思路，多个机器多机部署详见 [[分布式锁（很重要）#多机部署]]
需要说明的是，RedLock 比较重，看情况需不需要加
