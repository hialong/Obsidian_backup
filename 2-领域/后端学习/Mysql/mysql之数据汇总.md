---
created: 2024-02-08
updated: 2024-02-18
Type: knowledge
Status:
  - ⌛️ 等待
tags:
---
## 聚合函数
一些计算函数
```sql
--  max()
--  min()
--  AVG()
--  SUM()
--  COUNT()
SELECT min(invoice_total) from invoices;
-- distinct 去重
SELECT
	max( invoice_total ) AS highest,
	min( invoice_total ) AS lowest,
	AVG( invoice_total ) AS average,
	SUM( invoice_total ) AS total,
	COUNT( DISTINCT client_id ) AS total_records 
FROM
	invoices 
WHERE
	invoice_date > '2019-07-01';
```

- [x] mysql 看到了 p41, 稍微休息一下，整理一下，然后看完到 45 [【第五章】1- 聚合函数 | Aggregate Functions「汇总数据」\_哔哩哔哩\_bilibili](https://www.bilibili.com/video/BV1UE41147KC?p=41&vd_source=eb319c6e317591be75da0554d1d79e3a) 📅 2024-02-18 ✅ 2024-02-18


## GroupBy 语句 聚合搜索
### 单列分组
```sql
-- 查询表中所有数据的汇总
SELECT sum(invoice_total) as total_sales from invoices;
-- 查询表中所有数据的汇总,但是是按照client_id 一致的来汇总
SELECT client_id,sum(invoice_total) as total_sales from invoices GROUP BY client_id;
```

默认情况下，表中数据是按照 group by 的列来进行排序的，但是可以使用 order by **来进行二次排序** ![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240218225306.png)

>[!note] 语句执行过程
>FROM>WHERE>GROUP BY >SELECT >ORDER BY
>所以 GROUP BY 语句永远要在 where 语句之后，例如下面的
> ![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240218225637.png)


### 多列分组
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240218230429.png)

```sql
-- 按照state city的独一无二的排列进行聚合，配合sum进行汇总查询
SELECT state,city,sum(invoice_total) as total_sales from invoices i
JOIN clients USING (client_id)
GROUP BY state,city 
ORDER BY total_sales DESC;
```

- @ 
>[!example] 小练习
>按照时间和方法来聚合搜索当天的账单，并按照时间排序
>```sql
select date, pm. name as method, sum (amount) total_payments from payments 
join payment_methods pm  where payment_method = payment_method_id
GROUP BY date, payment_method
order by date;
>```



![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240218231428.png)

#### 总结 
- [ ] 多列分组的作用在于，会找到多列的所有的独一无二的排列做为分组依据，比如说，📅 2024-02-23 
## Having 关键字

>[!faq]- having 语句是用来干什么的？
>是用来在 groupBy 之后进行筛选的

- [x] mysql 看到 p43 [3- HAVING子句 | The HAVING Clause\_哔哩哔哩\_bilibili](https://www.bilibili.com/video/BV1UE41147KC/?p=43&spm_id_from=pageDriver&vd_source=eb319c6e317591be75da0554d1d79e3a) 📅 2024-02-22 ✅ 2024-02-22
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240222232036.png)
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240222232721.png)

```sql
SELECT  client_id, 
        sum(invoice_total) as total_sales,
        count(*) as number_of_invoices from invoices
GROUP BY 
    client_id
HAVING total_sales >500 AND number_of_invoices<6;
```
1. ~ 可以多条件排序，and 连接
2. & 但是 having 的条件必须在 select 里面存在，不能是别的字段，跟 where 不一样，where 可以使用任何列作为条件


## ROLLUP 运算符
- [ ] MySQL 44 课 [4- ROLLUP运算符 | The ROLLUP Operator\_哔哩哔哩\_bilibili](https://www.bilibili.com/video/BV1UE41147KC/?p=44&spm_id_from=pageDriver&vd_source=eb319c6e317591be75da0554d1d79e3a)📅 2024-02-23 
>[!note] 功能：
获得一个汇总行

![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240222233505.png)

1. & rollup 运算符号只能用于聚合值的列，比如这里的 sum 列