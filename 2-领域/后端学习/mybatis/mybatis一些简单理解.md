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

开启二级缓存后会先从二级缓存查找，找不到再去一级缓存去找，一级缓存没有数据再去数据库查询


**mybatis 缓存的本质就是在本地利用 map 来存储数据**
所以实际上这个在分布式环境下并不安全，本地的 map 实际上还是会出现脏数据或者数据对不上的问题

一级缓存默认开启，二级缓存通过 xml 配置开启（一般是并发环境防止脏数据开启的）
```xml
<mapper namespace="com.example.mapper.UserMapper">
    <!-- 开启二级缓存 -->
    <cache eviction="LRU" flushInterval="60000" size="512" readOnly="true"/>
</mapper>

```

建议生产上还是要结合 redis 等缓存来结合进行

## mybatis 执行的流程

整个执行流程分为以下几步
1. SqlSessionFactory 的创建，执行通过工厂创建 SqlSession 实例，SqlSessionFactory 是通过 SqlSessionFactory 构建的，通常是通过读取 MyBatis 配置文件进行初始化
2. 通过 OpenSession（）方法获取一个 SqlSession 对象
3. 执行，mybatis 根据方法的 id 获取不同的 sql 语句去执行
4. 命名空间和映射语句的查找。sql 映射语句根据 namespace 和 id 进行定位，Mybatis
- [ ] 看完流程
