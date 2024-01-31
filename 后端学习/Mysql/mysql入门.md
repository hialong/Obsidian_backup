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
- [*] 未完待续，课程看完了 p 8  [2- 选择子句 | The SELECT Clause\_哔哩哔哩\_bilibili](https://bilibili.com/video/BV1UE41147KC/?p=8&spm_id_from=pageDriver&vd_source=eb319c6e317591be75da0554d1d79e3a) 📅 2024-01-31
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
-- 模糊查询
select * from Customer where last_name like "%abc%" 
```

