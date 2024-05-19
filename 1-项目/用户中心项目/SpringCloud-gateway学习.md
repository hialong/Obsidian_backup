---
created: 2024-05-18
updated: 2024-05-18
Type: knowledge
Status: ⌛️ 等待
tags:
---
##  为什么要用

学习微服务，学习多个项目之间的整合，以及项目中 api 开放平台需要统计流量、流量染色、接口保护等能力

==注意，新建项目的时候，pom 文件很可能加的 gateway 的依赖带 mvc 的，这个两个是不一样的==
建议不用 mvc 版本的，因为 mvc 是阻塞 io，慢
[spring-cloud-starter-gateway-mvc和spring-cloud-starter-gateway的区别-CSDN博客](https://blog.csdn.net/weixin_46436257/article/details/135358258)
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240519000143.png)

## gateWay 能做什么？

简单来说，就是我可以通过访问网关的地址，而不是去访问原来的直接暴露的地址
让网关给我的请求加上必要的请求头、做一些校验，防止路径暴露、同时还能控制流量
### 网关可以实现哪些功能

1. 路由
2. 负载均衡
3. 统一鉴权
4. 处理跨域
5. 统一业务处理
6. 访问控制
7. 发布控制
8. 流量染色
9. 接口保护
    1. 限制请求
    2. 信息脱敏
    3. 降级熔断 
    4. 限流：令牌桶算法，漏桶算法，RedisLimitHandler
    5. 超时时间
10. 统一日志
11. 统一文档

- [ ] 学习令牌桶算法，漏桶算法，RedisLimitHandler
### 怎么配置？
具体可见官方文档
简单代码等：[Spring Cloud Gateway](https://spring.io/projects/spring-cloud-gateway)

详细配置信息
[Configuring Route Predicate Factories and Gateway Filter Factories :Spring Cloud Gateway](https://docs.spring.io/spring-cloud-gateway/reference/spring-cloud-gateway/configuring-route-predicate-factories-and-filter-factories.html)

官方文档讲的很清晰，简单的配置 yml 就可以做到很多过滤、跳转方面的能力 ![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240518235449.png)
添加请求头的能力
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240518235738.png)

### 全局过滤器

