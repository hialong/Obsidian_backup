---
created: 2024-12-29
updated: 2024-12-29
Type: knowledge
Status: ⌛️ 等待
tags:
---
## mybatis 的缓存

一级缓存和二级缓存
- 一级缓存是会话级别的缓存，即创建一个 Sqlsession 就是一个会话，一次会话可能会执行多次相同的查询，缓存可以重复利用之前查询的结果，用来提高性能
- 二级缓存是用来跨 sqlSession 的，一级缓存可能出现的问题在于
