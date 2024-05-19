---
created: 2024-05-19
updated: 2024-05-19
Type: knowledge
Status: ⌛️ 等待
tags:
---
## 背景

网关是一个单独的项目，但是统计这个流量什么的都需要用原本的 user-center 项目中的方法，或者直接自己操作数据库，有点太捞了，而如果变成 HTTP 的方式暴露出去呢，又太危险了，所以想到了 RPC 或者微服务或者直接按照之前的方法，打 jar 包给其他项目引用（sdk）

刚好，学习一下微服务，所以决定选择使用微服务的Dubbo

## Dubbo （RPC）
 -- **像调用本地方法一样调用远程方法**
 
http 协议有 7 层，RBC 的性能更高，可以支持其他的协议，不用 7 层（例如 TCP 等）

那么 RBC 的实现框架目前比较好的有很多，比如 Dubbo
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240519201926.png)
[Dubbo 入门 | Apache Dubbo](https://cn.dubbo.apache.org/zh-cn/overview/quickstart/)

阅读官方文档是最快的学习的方法

Dubbo 框架中提供一个 dubbo-zookeeper 作为注册中心，根据他的入门可以很方便的看到项目是如何启动的，springBoot 调用方法非常简单，一个消费者，一个公共接口类，一个提供者 ![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240519202713.png)

### 为什么选 Dubbo

首先这个文档是中文，好看，好学
其次，fein 是 Http 协议的，这个 dubbo 是 Rpc 的用的 Triple 协议，更高的性能
然后 Dubbo 好学

### 怎么用

我现在的需求是 gateWay 中能调用 user-center 这个里面封装的方法，也就是 gate-way 是消费者，user-center 是提供者，还需要一个公共的接口包，提供 interface 接口


- @ 重要提示
==首先，保证你的项目不要在**中文路径**下==
==首先，保证你的项目不要在**中文路径**下==
==首先，保证你的项目不要在**中文路径**下==
**重要的话说三遍**
- @ 

里面的注册中心有两种，一种是 zookeeper 一种是 nacos ，用哪种？
根据 AI 回答
**Dubbo 3. x 版本对 Nacos 的支持更为友好和直接，如果您的项目使用的是较新的 Dubbo 版本，可能会倾向于选择 Nacos 作为注册中心。**

Zookeeper 优点:
成熟稳定：Zookeeper 是 Apache 的顶级项目，历史悠久，广泛应用于大型分布式系统中，稳定性经过了长时间的验证。
一致性保证：采用 ZAB（Zookeeper Atomic Broadcast）协议，能够保证强一致性（CP 模型），适合那些对数据一致性要求极高的场景。
生态丰富：与 Hadoop、Kafka 等众多大数据组件集成紧密，支持广泛的开源生态系统。
多用途：除了服务发现和配置管理外，还常用于分布式锁、队列、选举等场景。
Zookeeper 缺点:
复杂度较高：配置和运维相对复杂，对于新手来说学习曲线较陡峭。
资源消耗：在小规模集群中资源占用相对较高，可能不适合轻量级应用。
单点问题：虽然设计为高可用集群，但在实际部署中若配置不当，仍有可能出现单点故障。
数据模型限制：数据模型基于树形结构，对于某些特定类型的数据存储和查询可能不够灵活。
Nacos 优点:
易用性：Nacos 设计时考虑了易用性，提供了图形化界面，便于管理和监控，上手更快。
一体化解决方案：集成了服务发现、配置管理、健康检查等功能于一体，简化了微服务架构的搭建。
动态配置：依赖 MySQL 等关系型数据库存储配置，支持实时配置更新和推送，无需重启服务即可生效。
AP 设计：默认支持 AP 模式（可用性优先），更适合现代微服务架构的需求，能在网络分区时保证服务可用性。
与 Spring Cloud/Dubbo 等框架集成良好：特别适合微服务架构，特别是与 Spring Cloud 生态结合紧密。
Nacos 缺点:
依赖外部数据库：虽然增加了灵活性和持久化能力，但也引入了额外的运维成本和潜在的单点故障风险。
性能和扩展性：在极端高并发或大规模集群环境下，相对于 Zookeeper，Nacos 的性能和扩展能力可能需要更细致的调优。
成熟度：相比于 Zookeeper，Nacos 相对较新，虽然发展迅速，但在某些特定领域的成熟度和社区支持可能不如 Zookeeper。
综上所述，在选择 Zookeeper 还是 Nacos 时，应根据具体需求、团队熟悉度以及项目的长远规划来决定。Dubbo（您提供的版本为 3.1.7）作为服务治理框架，可以与两者集成，但需要注意的是，Dubbo 3. x 版本对 Nacos 的支持更为友好和直接，如果您的项目使用的是较新的 Dubbo 版本，可能会倾向于选择 Nacos 作为注册中心。

==所以咱们用 nacos==  **后面有需要再去学 zookeeper**

### 对于现有的项目，应该如何引入？
首先这个时候就不要去看 Dubbo 的官方文档，因为 Dubbo 的官方文档写的是用的 zookeeper 的配置，搜 Nacos，用 Nacos 做注册中心
[Dubbo 融合 Nacos 成为注册中心](https://nacos.io/zh-cn/docs/use-nacos-with-dubbo.html)

然后发现这个是 nacos 1.x 的，应该去看 nacos2.x 的，这个是最新的，看到里面说 nacos 里面 issue 更新了新的东西，不在强制依赖了 [Dubbo is not compatible with Nacos2 client. · Issue #7291 · apache/dubbo · GitHub](https://github.com/apache/dubbo/issues/7291) （所以用 Dubbo2 的应该也能用，dubbo 3 我先试试）

#### 下载 nacos 2
nacos 2.3.2 版本，官方文档直接下
[Releases · alibaba/nacos](https://github.com/alibaba/nacos/releases)

按照官方文档启动后[Nacos 快速开始](https://nacos.io/zh-cn/docs/v2/quickstart/quick-start.html)
这个地址就是 nacos 的地址
http://localhost:8848/nacos/#/configurationManagement?dataId=&group=&appName=&namespace=&pageSize=&pageNo=
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240519231558.png)
