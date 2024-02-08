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
