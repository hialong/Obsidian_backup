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

- [ ]  mysql 看到了 p41, 稍微休息一下，整理一下，然后看完到 45 [【第五章】1- 聚合函数 | Aggregate Functions「汇总数据」\_哔哩哔哩\_bilibili](https://www.bilibili.com/video/BV1UE41147KC?p=41&vd_source=eb319c6e317591be75da0554d1d79e3a)




