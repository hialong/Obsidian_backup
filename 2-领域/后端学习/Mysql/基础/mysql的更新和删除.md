---
created: 2024-02-08
updated: 2024-02-08
Type: knowledge
Status: ⌛️ 等待
tags:
---
## 列属性
1. varchar 和 char 的区别，varchar 代表了变化，如果在 mysql 的列中，字段是 varchar （50）但是你存储进去的字符串大小只有 5，那么 mysql 就会用直接存储这个 5 字符串大小的字符串，但是如果你的列属性是 CHAR(50), 那么 MySQL 会再插入 45 个字符来满足 CHAR（50）
2. 默认值，不给值会得到默认值
3. not null 属性
4. 对于 mysql 来说，**单引号**包裹的字符串和**双引号**包裹的没区别

## 插入

### 默认值关键字 DEFAULT
```sql
-- 默认值插入，单引号和双引号没有区别
INSERT INTO customers
VALUES
	(
		DEFAULT,
		'john',
		'Smith',
		"1990-01-01",
		NULL,
		'address',
		'city',
		'CA',
		DEFAULT);
-- 或者你就直接insert into后面加列名，省略默认值就行
```

### 插入多行
```sql
-- value 后面逗号隔开就行了
insert into shippers (name)
values 
('shippers1'),
('shippers2'),
('shippers3')
```

### 插入多表
使用 mysql 里面内置的函数 `LAST_INSERT_ID()`，可以获取上一次我们插入的 id 
```sql
SELECT*FROM orders;-- 模拟一次添加，1号顾客下单
INSERT INTO orders (customer_id,order_date,STATUS) VALUES (1,'2019-01-02',1);
SELECT LAST_INSERT_ID();
-- 使用内置函数LAST_INSERT_ID() 进行插入
INSERT INTO order_items VALUES (LAST_INSERT_ID(),1,1,2.95),(LAST_INSERT_ID(),2,1,3.95)
```

## 创建表复制
>[!note] 创建表复制
>我们可以很简单的用下面的语句创建一个表复制
>```sql
>-- 创建表复制
>create table orders_archived as select * from orders;
>```
>但是，需要注意的是，用这个方式创建表复制的时候，会丢失主键和自增的列属性
>另外就是我们创建了这张表之后，因为表的字段完全一致，后面我们可以使用一些类似下面的操作来插入表 [[#子查询]]
>```sql
>INSERT into orders_archived
SELECT * from orders where order_date <'2019-01-01';
>```


## 子查询
实际上就是插入或者更新的时候，插入的信息用 select 方式获取
例如
```sql
UPDATE invoices 
SET payment_total = invoice_total * 0.5,
payment_date = due_date 
WHERE
	client_id = (
	SELECT
		client_id 
	FROM
		clients 
WHERE
	NAME = 'Myworks')
```
**注意**上面的查询如果子查询的 select 是返回多个的话，我们就要用 `update set xxx in ` 了，就是加上 `in` a 关键字


## 总结
这里我们学到了 sql 语法中的更新操作，有以下几个部分需要注意
1. 我们可以使用 DEFAULT 关键字表示插入默认的值
2. 我们可以用 `LAST_INSERT_ID()` 表示上一次插入的 id
3. 子查询，这个可以用到很多地方，就包括创建表复制