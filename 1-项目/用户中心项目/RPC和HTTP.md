---
created: 2024-05-19
updated: 2024-05-19
Type: knowledge
Status: ⌛️ 等待
tags:
---
## 背景

网关是一个单独的项目，但是统计这个流量什么的都需要用原本的 user-center 项目中的方法，或者直接自己操作数据库，有点太捞了，而如果变成 HTTP 的方式暴露出去呢，又太危险了，所以想到了 RPC 或者微服务或者直接按照之前的方法，打 jar 包给其他项目引用（sdk）

## RPC
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