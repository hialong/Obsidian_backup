---
created: 2024-01-21
updated: 2024-01-31
Type: knowledge
Status: ⌛️ 等待
tags:
---
## mysql 基础语法

```sql
-- 去重多余结果
select distinct state from customer
```
- [x] 未完待续，课程看完了 p 8  [2- 选择子句 | The SELECT Clause\_哔哩哔哩\_bilibili](https://bilibili.com/video/BV1UE41147KC/?p=8&spm_id_from=pageDriver&vd_source=eb319c6e317591be75da0554d1d79e3a)，注：新看完了8课 📅 2024-01-31 ✅ 2024-01-31
```sql
-- 查询语法
select * from xxx
select * from xxx where  a>b and cday > '1990-01-01'

```
1. 运算符顺序 (`+-*/`)，乘除大于加减（可以括号改变顺序）
2. 逻辑符顺序（and, or） and 运算优先评估（可以括号改变顺序）
3. 时间可以直接写

```sql
-- in语法
select * from Customer where state in ('va','la','ga')
-- between语法
select * from Customer where points between 1000 and 3000
select * from Customer where birthday between '1990-01-01' and '2000-01-01'

```

### 模糊查询
1. 可以用 `%` 来匹配任意长的字符，
2. 用 `_` 匹配单个字符
```sql
-- 模糊查询
select * from Customer where last_name like "%abc%" 
select * from Customer where last_name like "a____c" 
```

### 正则表达式搜索
一个新的关键字，正则 `REGEXP`
```sql
-- 下面两个表达等价
select * from customer where last_name like '%field%'
select * from customer where last_name regexp 'field'

```
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240131212858.png)

关于正则
1. 使用 `^xx` 表示以 `xx` 开头 
2. 使用 `xx$` 表示以 `xx` 结尾
3. 使用 `a|b` 表示或条件
4. 使用 `[abcx]e` 表示中括号里面的任意字母均可以出现在 e 前面，例如 `ae`、`be`、`xe`

![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240131214544.png)
例子：
```sql
--查询以field结尾或包含mac或以rose开头的
SELECT * from customer where last_name regexp 'field$|mac|^rose';
SELECT * from customers where last_name regexp '(field|mac)$';
```
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240131214045.png)

```sql
-- 正则查法
SELECT * FROM customers WHERE LAST_NAME REGEXP '[a-h]e';
```

### 查询 null 数据 IS NULL
```SQL
SELECT * from customers where phone IS NULL
```

### 查询排序
`order by` 按顺序排序
```sql
-- 多列排序，先排前面的，再排后面的
SELECT  * FROM customers ORDER BY state desc, first_name desc;
-- 排序不需要在sql查询的列里
SELECT  first_name,last_name FROM customers ORDER BY state desc, first_name desc;
-- 可以用别名排序，或者加的一个列
SELECT  first_name,last_name, 10 as points FROM customers ORDER BY points, state desc;
-- 可以用列来排序，这里的1，2指的是查询的first_name last_name,但是尽量避免这种写法，防止前面查询的顺序会变换导致查询的顺序变化
SELECT  first_name,last_name, 10 as points FROM customers ORDER BY 1,2;
-- 他也可以是一个表达式
SELECT *,quantity * unit_price as total_price from order_items ORDER BY quantity * unit_price;
SELECT *,quantity * unit_price as total_price from order_items ORDER BY total_price;
```

- [x] sql 看完 p16 下次看 p17 了 [11- LIMIT子句 | The LIMIT Clause\_哔哩哔哩\_bilibili](https://www.bilibili.com/video/BV1UE41147KC/?p=17&spm_id_from=pageDriver&vd_source=eb319c6e317591be75da0554d1d79e3a) 📅 2024-02-04 ✅ 2024-02-04

### limit 的用法

```sql
-- 前面相当于offset 6  往后查三位
select * from customers limit 6,3
```


### JOIN 关键字
#### 单 join 另一张表

```sql
-- 单join
select * from orders JOIN customers on orders.customer_id = customers.customer_id

```

1. 如果搜索的是**两张表里面的共有列**会报错：列不明确。例如 `select customer_id，order_id from orders JOIN customers on orders.customer_id = customers.customer_id` 正确的写法应该是给 customer_id 加上明确的表名
2. 可以给表赋别名，但是赋别名之后就必须要用别名了，否则报错，就像这样 ![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240204232449.png)

#### 自连接，join 自己
```sql
USE sql_hr;
SELECT e.employee_id,e.first_name,m.first_name as manger FROM employees e JOIN employees m ON e.reports_to=m.employee_id;
```
- [ ] mysql 看 21 [4- 多表连接 | Joining Multiple Tables\_哔哩哔哩\_bilibili](https://www.bilibili.com/video/BV1UE41147KC/?p=21&spm_id_from=pageDriver&vd_source=eb319c6e317591be75da0554d1d79e3a) ⏫ 📅 2024-02-05
#### 多表连接

### 多数据库连接
只需要给查询的数据前面加上数据库的名字就可以了