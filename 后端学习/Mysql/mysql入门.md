---
created: 2024-01-21
updated: 2024-01-31
Type: knowledge
Status: ⌛️ 等待
tags:
---
## mysql 基础语法一

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
#### 内连接
内连接是 inner join 但是实际上，**这个 inner 可有可无**
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
- [x] mysql 看 21 [4- 多表连接 | Joining Multiple Tables\_哔哩哔哩\_bilibili](https://www.bilibili.com/video/BV1UE41147KC/?p=21&spm_id_from=pageDriver&vd_source=eb319c6e317591be75da0554d1d79e3a) ⏫ 📅 2024-02-06 ✅ 2024-02-06
#### 外连接
mysql 外连接有两种，一个是 left join  一个是 right join
==外连接也是，存在 `left outer join` 和 `right out join` 但是和 inner join 一样，这个 outer 可有可无 ==
**区别在于**，join 是内连接，查询到的数据不满足条件的，不会被返回
==而 left join 或者 right join  会返回所有的左边（右边）的表的数据==
举个例子
```sql
SELECT c.customer_id,c.first_name,o.order_id from customers c join orders o on c.customer_id = o.customer_id order by c.customer_id;
```
可以看到，单个 join 查询返回的数据，如果没有满足 on 后面的条件，那么就会不会出现在返回数据里面，那如果我们使用 left join 呢？
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240206224340.png)
可以看到，leftjoin 返回了左边表的所有的数据，即便是不满足条件（没有 orderid 与之对应）
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240206224647.png)

### 多数据库连接
只需要给查询的数据前面加上数据库的名字就可以了

### 复合连接条件
目前看是就是join多加了一个条件，需要在 where 语句后面进行连接上 and 就行

### 隐式连接
下面是隐式连接的写法，但是一旦忘记写 where 语句，那么，会造成交叉连接，推荐使用显式连接法，这样有问题会报错
```sql
-- 显式语法
SELECT * from orders o JOIN customers c ON o.customer_id = c.customer_id;

-- 隐式连接
SELECT * from orders o,customers c WHERE o.customer_id = c.customer_id;
-- 会造成交叉连接，会出现异常多的数据查询
SELECT * from orders o,customers c ;
```

- [ ] sql 未完待续，下次看 P25 [8- 多表外连接 | Outer Join Between Multiple Tables](https://www.bilibili.com/video/BV1UE41147KC?p=25&vd_source=eb319c6e317591be75da0554d1d79e3a)
## 总结
对以上学习进行一个小总结

1. 在模糊查询中，`%` 表示任意长的字符，而 `_` 表示单个字符
2. 正则关键字 `regexp` 可以直接替代模糊查询的 like 关键字，另外就是正则查询的一些写法，简单来说就是
	1.  使用 `^xx` 表示以 `xx` 开头 
	2. 使用 `xx$` 表示以 `xx` 结尾
	3. 使用 `a|b` 表示**或**条件
	4. 使用 `[abcx]e` 表示中括号里面的任意字母均可以出现在 e 前面，例如 `ae`、`be`、`xe`
3. 查询 null 的时候要用 is null 用= null 是搞不出来的，这个我在工作中犯过错
4. `order by` 的排序能力很强，不仅仅能排一个列 [[#查询排序]]
	1. 如果排两个条件，就先排前面的条件，后排后面的条件，而且条件不需要出现在查询的列里面
	2. 他的排序条件甚至不需要是表里面的列，你 select 一个常数，用别名都能用来排序
	3. 可以用 `order by 1,2` 这种语法来表示使用查询中的第一第二列作为排序条件
5. `limit` 关键字的用法，除了 `limit 6` 这种单纯的限制返回数量，还可以做到 offerset 的效果，如 `limit 3,10` ，相当于往后数三位后再限制返回 10 个
6. `JOIN` 关键字，分为内连接 `inner join` 和两个外连接 `left outer join` 和 `right outer join` (inner 和 outer 都可以省略)，内连接和外连接最大的不同就是 [[#JOIN 关键字]]
	1. 内连接如果没有满足的条件，就会不返回，
	2. 外连接，以左连接为例，会返回所有的左边的表的信息，以及满足 on 后面条件的右边的表的信息
- [ ]  看完 p30 再做一遍总结，到时候刚好第三章看完