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
- 二级缓存是用来跨 sqlSession 的，一级缓存可能出现的问题在于脏数据的出现，由于一级缓存的隔离，不同 SqlSession 之间的修改不会影响彼此，比如 SqlSession 1 读了数据 A，SqlSession 2 将数据改为 B，此时 SqlSession 1 还是得到 A，这就出现了脏数据的问题。所以二级缓存的开启会对同一个命名空间下的全部都产生影响

开启二级缓存后会先从二级缓存查找，找不到zao
