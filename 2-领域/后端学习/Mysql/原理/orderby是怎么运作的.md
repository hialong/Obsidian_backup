---
created: 2024-02-29
updated: 2024-02-29
Type: knowledge
Status: ⌛️ 等待
tags:
---
##  全字段排序
我们在查询中经常会使用到关键字 `order by` 用来排序，那么他的底层原理是什么样的呢？
我们可以举个例子来说明

首先创建一张表

```sql
CREATE TABLE 't' 
( 'id' int(11) NOT NULL, 
'city' varchar(16) NOT NULL, 
'name' varchar(16) NOT NULL, 
'age' int(11) NOT NULL, 
'addr' varchar(128) DEFAULT NULL, 
PRIMARY KEY ('id'), 
KEY 'city' ('city') ) ENGINE=InnoDB;
```

这样就创建了一张城市姓名表，表中的主键是"id"列，该列的值是唯一的。同时还创建了一个"city"列的索引。

我们的 sql 这样写，利用了 city 的索引，避免了全表扫描
```sql
select city,name,age from t where city='杭州' order by name limit 1000 ;
```

- ~ 使用 explain 命令查看这个语句的执行情况

![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240304234707.png)

主要看 extra 里面的字段![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240304234818.png)
这个 Using filesort 表示的就是需要排序，Mysql 会给每个线程**分配一块内存用于排序**称之为，sort_buffer

下面是 city 的索引示意图
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240304235828.png)
可以看到满足 city = '杭州' 的是 IDX 到 ID(X+N) 这些记录

通常情况下，语句执行流程如下

1. 初始化 sort_buffer，确定放入 name、city、age 这三个字段；
2. 从索引 city 找到第一个满足 city=‘杭州’条件的主键 id，也就是图中的 ID_X；
3. 到主键 id 索引取出整行，取 name、city、age 三个字段的值，**存入 sort_buffer 中**；
4. 从索引 city 取**下一个记录**的主键 id；
5. 重复步骤 3、4 直到 city 的值不满足查询条件为止，对应的主键 id 也就是图中的 ID_Y；
6. 对 sort_buffer 中的数据按照字段 name 做快速排序；
7. 按照排序结果**取前 1000 行**返回给客户端。

暂且就把这个排序称之为全字段排序 ![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240305000721.png)

### 排序动作的执行地

上面我们看到我们是在 sort_buffer 中按照 name 进行排序的，这个排序的动作，可能在内存中完成，也可能需要使用外部排序，这个取决于，排序所需的内存和参数 sort_buffer_size 

sort_buffer_size 就是 mysql 为排序开辟的内存（sort_buffer）的大小，这个看需要排序的数据量
- ~ 如果要排序的数据量小于 sort_buffer_size，排序就在内存中完成。
- ~ 但如果排序数据量太大，内存放不下，则不得不利用磁盘临时文件辅助排序。


可以通过下面的方式确定一个排序语句是否使用了查询
```sql
/* 打开 optimizer_trace，只对本线程有效 */
SET optimizer_trace='enabled=on';
/* @a 保存 Innodb_rows_read 的初始值 */
select VARIABLE_VALUE into @a from performance_schema.session_status where variable_name = 'Innodb_rows_read';
/* 执行语句 */
select city, name,age from t where city='杭州' order by name limit 1000;
/* 查看 OPTIMIZER_TRACE 输出 */
SELECT * FROM `information_schema`.`OPTIMIZER_TRACE`\G 
/* @b 保存 Innodb_rows_read 的当前值 */
select VARIABLE_VALUE into @b from performance_schema.session_status where variable_name = 'Innodb_rows_read';
/* 计算 Innodb_rows_read 差值 */
select @b-@a;

```

- [ ] 未完待续orderby