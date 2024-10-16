---
created: 2024-10-16
updated: 2024-10-16
Type: knowledge
Status: ⌛️ 等待
tags:
---
## 单表是怎执行查询语句的？
单独表的查询查询的名称 
    1. 单表怎么执行查询语句？  
    2. 通过主键或者‘唯一’二级索引列与常数的等值比较来定位一条记录是像坐火箭一样快的 这种访问方法定义为：const  
    3. 由于普通二级索引并不限制索引列值的唯一性（唯一索引则会限制索引的唯一值，查询更快，因为唯一索引只能匹配唯一一个id再回表，但是要注意二级唯一索引为null的情况，特殊情况）  
    4. 采用二级索引来执行查询的访问方法称为：ref  
    5. 不论是普通的二级索引，还是唯一二级索引，它们的索引列对包含NULL值的数量并不限制，所以我们采用key IS NULL这种形式的搜索条件最多只能使用ref的访问方法，而不是const的访问方法。  
    6. 利用索引进行范围匹配的访问方法称之为：range。  
    7. 直接遍历二级索引比直接遍历聚簇索引的成本要小很多，设计MySQL的大佬就把这种采用遍历二级索引记录的执行方式称之为：index  
    8. 全表扫描就是all  
    9. 使用到多个索引来完成一次查询的执行方法称之为：index merge  
    10. MySQL在某些特定的情况下才可能会使用到Intersection索引合并：  
        1. 二级索引列是等值匹配的情况，对于联合索引来说，在联合索引中的每个列都必须等值匹配，不能出现只匹配部分列的情况  
        2. 主键列可以是范围匹配

## 联合索引的一些补充
关于联合索引，需要满足最左原则，不然是不走索引的
这取决于你的查询条件和表的索引结构。假设你有一个表 example_table，并且在该表上有以下索引：  
index 1：联合索引 (col 1, col 2)
index 2：单列索引 (col 3)


```sql
-- 使用 index1，因为查询条件包含 col1
SELECT * FROM example_table WHERE col1 = 'value1';  
```

```sql
-- 使用 index1，因为查询条件包含 col1 和 col2
SELECT * FROM example_table WHERE col1 = 'value1' AND col2 = 'value2';  
```
  
```sql
-- 使用 index2，因为查询条件包含 col3
SELECT * FROM example_table WHERE col3 = 'value3';  
```
  

  ```sql
  -- 可能会使用 index1 或 index2，具体取决于数据库优化器的选择  
  SELECT * FROM example_table WHERE col1 = 'value1' AND col3 = 'value3'; 
  ```
 
  ```sql
  -- 全表扫描，因为查询条件不包含任何索引列  
  SELECT * FROM example_table WHERE col4 = 'value4';
  ```

```sql
-- 也会进行全表扫描，因为查询条件中不包含联合索引的第一个列 col1
SELECT * FROM example_table WHERE col2 = 'value4'; 
```
