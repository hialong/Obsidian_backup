---
created: 2024-03-12
updated: 2024-03-12
Type: knowledge
Status: ⌛️ 等待
tags:
---
##  count(`*`) 和 count(1) 有什么区别？哪个性能最好？

直接给结论 ![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240312221044.png)

## Count（）是什么

- ~ count 其实就可以理解成一个用来统计不为 null 的字段有多少个的一个函数

如下，
```sql
select count(name) from t_order;
```
这句话的意思就是统计这张表里面，**name 字段不为 null 的记录有多少个**


那么 count（1）呢？
```sql
select count(1) from t_order;
```
1 其实就永远不为 null 的，所以这个意思就变成了，使得 1 不为 null 的字段有多少个，因为 1 永远不为 null，实际上是常数，所以，**实际上这查询的是表里面所有记录的个数**
实际上，数字啥的常量都是一样的![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240312221804.png)


## count（主键）的执行过程

- [ ] Mysql 的 server 层是啥

执行过程就是一个循环的读取过程，在通过 count 函数统计多少个记录的时候，
1. Mysql 的 **server 层**会维护一个叫 count 的变量
2. server 层会循环向 innoDB 读取记录，如果 count 函数指定的参数不为 null，那么就会将 count 加一，最后返回 


count (**id**) 的执行过程大致分为两种情况，索引看[[索引]]
1. 只有聚簇索引，也就是只有主键索引
	1. @ 如果只有主键索引的话，那么 server 层查询就会去查主键索引，循环遍历主键索引![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240312223337.png)
2. 有二级索引，也就是非主键索引
	1. @ 那么InnoDB 循环遍历的对象就不是聚簇索引，而是二级索引![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240312223437.png)


原因在于，mysql 的索引，每个索引都是一颗 B+树 [[索引#InnoDB 的索引模型]]，**主键索引里面储存的是当前行，而二级索引储存的是 id 值，所以二级索引更小，遍历二级索引的 IO 成本更低**

## count（1）执行过程是什么样子的？

与上面 count（**主键**）类似，也是 server 层循环查询对应的字段，但是相对于 count（**主键**），count（1）里面的数是常数，所以不需要判断数据是否为空，所以，**执行过程不会读取任何字段的值**，这就解释了为什么 count（1）的效率略高于 count（**主键**）

其余和主键索引类似，有二级索引遍历二级索引，没有就遍历主键索引

## count（`*`）的执行过程

看到 `*` 这个字符的时候，是不是大家觉得是读取记录中的所有字段值？

对于 `selete *` 这条语句来说是这个意思，但是在 count(*) 中并不是这个意思。

**count(`*`) 其实等于 count(`0`)**，也就是说，当你使用 count(`*`) 时，MySQL 会将 `*` 参数转化为参数 0 来处理。

而且 MySQL 会对 count(`*`) 和 count(1) 有个优化，如果有多个二级索引的时候，优化器会使用key_len 最小的二级索引进行扫描。

只有当没有二级索引的时候，才会采用主键索引来进行统计。

## count（**字段**）执行过程
这个查询方式的效率是最慢的![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240312224537.png)
可以看到，这是全表扫描的方式查的，（type 是 ALL 就说明是全表扫描 ）


## 其他要注意的
1. Mysql 的另一个引擎 MyISAM 查询 count（`*`）会更快，时间复杂度只有 O（1）原因在于他维护了一个 meta 信息，储存了 row_count 的值，由表级锁来保证一致性
2. 而 InnoDB 是支持事务的，同一时刻的多个查询，由于多版本并发控制（MVCC）, innoDB 也不知道需要返回多少数据，所以**只能循环遍历**
3. 如果都带了 where 进行过滤，那两个查询 count 的方式就没区别了，都要扫描全表

### 优化 COUNT(`*`)

简单来说，使用 explain 语句来代替 count（`*`）就能得到近似值，因为 explain 语句不需要真正的查询，只是估计
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240312225820.png)
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240312225829.png)

或者专门出一张表来维护所有的记录，每次插入删除的时候，都要操作这张表来记录对应的数据，这个比较麻烦

## 总结

结合实际工作，我理解是这样
1. @ 首先是，如果需要计数，写在 sql 里面的话，尽量给表配上一个二级索引
2. @ 如果一定要查某个字段不为 null 的个数的话，就给这个字段加上一个二级索引


