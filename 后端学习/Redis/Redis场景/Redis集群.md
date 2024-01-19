---
Created: 2024-01-17
Updated: 2024-01-17
Type: knowledge
Status: 🌱 未完成
截止日期: 
目标: 
领域: 
Tags:
---
### 集群是什么
对 Redis 集群来说，集群就是多个 Redis 合作解决问题，一起对外提供服务，但对外看起来是一个单机服务的方案

### 部署集群
我们可以从例子实际出发
#### 启动 3 个 Redis 服务
首先创建一个目录，redisCluster ，里面创建三个子目录 7000，7001，7002，这些目录都 copy 一份 redis.conf 配置，在其中修改下面 3 个字段：
1. cluster-enabled，设置为 yes，这样才能成为集群的一份子 
2. port，修改为为对应数字，比如 7000 目录 port 就改为 7000
3. dir 改为./，表示数据放在当前目录下
```Bash
# Normal Redis instances can't be part of a Redis Cluster; only no
# started as cluster nodes can. In order to start a Redis instance
# cluster node enable the cluster support uncommenting the folLowi
#
cluster-enabled yes
# Accept connections on the specified port, default is 6379 （IANA
# If port θ is specified Redis will not Listen on a TcP socket.
port 7000
# Note that you must specify a directory here, not a file name.
dir ./
```

然后在 7000 7001 7002 目录下，使用如下命令
```bash
redis-serverredis.conf
```

执行之后目录结构如下
```bash
|--- 7000
|	|---appendonly.aof
|	|---nodes.conf
|	└─--redis.conf
|	
|--- 7001
|	|---appendonly.aof
|	|---nodes.conf
|	└─--redis.conf
└─-- 7002
	|---appendonly.aof
	|---nodes.conf
	└─--redis.conf

```

接着使用命令 `redis-cli -p 7000` 连接上 7000 节点，然后 `cluster nodes` 查看节点，只有一个节点在集群中
```bash
127.0.0.1:7000> cluster nodes
f48115efb65569d0740b29a708e6de9e1ae439b6:7000@17000myse1f,master
```

#### 连接 Redis 服务, 组建集群！

组建集群的命令是 `cluster meet`
```bash
127.0.0.1:7000>clustermeet 127.0.0.1 7001
OK
127.0.0.1:7000>cluster meet 127.0.0.1 7002
OK
```
这样就连接上了 7001 和 7002 了，再在 7000 的客户端查询 `cluster nodes`
```bash
127.0.0.1:7000> cluster nodes
f07a9325cd3bc65bab81ba80246552308bd019dd 127.0.0.1:7001@17001 master - 0 1678499786966 1 connected
f48115efb65569d0740b29a708e6de9e1ae439b6 127.0.0.1:7000@17001 myse1f,master - 0 167849978500 0 connected
5a12b48f3fb9d057a0da4bb32fee07b6912e7034 127.0.0.1:7002@17001 master -0 1678499785956 2 connected
```

但是这时候还需要做一步 **分配 hash 槽** 详见[[RedisHash槽]]
	连接到集群后，我们还需要为每个节点分配==责任区间==，也就是每个节点对应的数据区间
- [x] Redis hash 槽要连接到这块 ✅ 2024-01-18

```bash
redis-cli -p 7000 cluster addslots {0..5461}
redis-cli -p 7001 cluster addslots {5462..10922}
redis-cli -p 7002 cluster addslots {10923..16383}
```

然后再查询 `cluster nodes` 就能看到到各自的数据区间了
```bash
127.0.0.1:7000> cluster nodes
f07a9325cd3bc65bab81ba80246552308bd019dd 127.0.0.1:7001@17001 master - 0 1678499786966 1 connected 5462-10922
f48115efb65569d0740b29a708e6de9e1ae439b6 127.0.0.1:7000@17001 myse1f,master - 0 167849978500 0 connected 0-5461
5a12b48f3fb9d057a0da4bb32fee07b6912e7034 127.0.0.1:7002@17001 master -0 1678499785956 2 connected 10923-16383
```
#### 操作集群
直接操作 7000 的客户端就行，如果不在对应的 slot，就会让你 MOVED，Redis 怎么判断出 MOVED 错误的可以看这里 [[RedisHash槽#储存槽的位置]]
	GPT 解释: 在 Redis 集群环境中，"MOVED"错误是一个常见的响应，它告诉客户端请求的键 (key)存在于另一个不同的节点。Redis 集群通过分片（sharding）来实现数据的水平扩展，这种方式将所有数据分散存储在多个节点上，每个节点负责维护一部分键空间。Redis 集群将所有的键空间分成了 16384 个槽（slots)，每个键通过 HASH_SLOT 算法映射到一个槽，每个节点负责一部分槽的管理。当客户端尝试访问一个键时，它会根据该键计算出相应的槽，并将请求发送到负责该槽的节点。如果客户端尝试访问的键不在当前节点负责的槽内，那么节点会返回一个"MOVED"错误，并指示正确负责该键的节点和槽。"MOVED"错误的格式通常如下：`MOVED <slot> <host>:<port><slot>` 是键映射到的槽的编号。`<host>` 是负责该槽的节点的 IP 地址。`<port>` 是负责该槽的节点的端口号。
```bash
127.0.0.1:7000>get bigfish
(error) M0VED 12574 127.0.0.1:7002
127.0.0.1:7000>get bigcat
(error) M0VED 10548 127.0.0.1:7001
127.0.0.1:7000>get bigdog
(error)M0VED11769127.0.0.1:7002
127.0.0.1:7000>get bigmouse
(error) M0VED 11002 127.0.0.1:7002
127.0.0.1:7000>get bigmonkey
(error)MOVED9888127.0.0.1:7001
127.0.0.1:7000>get bigdragon
(error)MOVED10224127.0.0.1:7001
127.0.0.1:7000>get bigsnake
(nil)
127.0.0.1:7000> set bigsnake snake
OK
127.0.0.1:7000> get bigsnake
```
具体的操作如上

#### 添加新节点
集群的操作如果想要新添加节点的话应该怎么做呢？hash 槽已经分配完的情况下，怎么获取新的 hash 槽呢？
- [x] 未完待续 ✅ 2024-01-18
第一步先起一个 Redis, 然后加入集群
```bash
127.0.0.1:7000>clustermeet 127.0.0.1 7003
OK
```
然而此时，新加入的节点还没有负责任何槽，使用 `cluster nodes` 可以看到
```bash
127.0.0.1:7000> cluster nodes
f07a9325cd3bc65bab81ba80246552308bd019dd 127.0.0.1:7001@17001 master - 0 1678499786966 1 connected 5462-10922
f48115efb65569d0740b29a708e6de9e1ae439b6 127.0.0.1:7000@17001 myse1f,master - 0 167849978500 0 connected 0-5461
5a12b48f3fb9d057a0da4bb32fee07b6912e7064 127.0.0.1:7003@17001 master -0 1678499786400 0 connected 
5a12b48f3fb9d057a0da4bb32fee07b6912e7034 127.0.0.1:7002@17001 master -0 1678499785956 2 connected 10923-16383
```
他没有负责任何槽位（这里以实际操作返回为准，只要知道他没返回槽位就行了）

第二步，分配一些槽位给这个节点命令为 `redis-cli --cluster rehard 127.0.0.1:7000`
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240118231943.png)
这里的意思是，从 7000 和 7001转移了一共 100 个 slot 给 7003
于是从 7000 和 7001，分别拿了 50 个 slots 给 7003，这里分配的就是节点各自按编号最小的 50 个左右 slot，比如 7000，就是 0-50。
完成之后 ![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240118232427.png)

## 总结
>[!question] 集群是什么？
>	集群是指多个 Redis 共同工作，分配不同的 hash 槽的区域，但是对外总体是暴露一个单独的服务的方案

>[!question] 怎么访问集群
>访问的话，就是随便找一台已经在集群里面的客户端，直接用就行了，get 不到的话会有 moveD 报错，告诉你去找哪个集群，使用 cluster node 查看集群

>[!question] Redis 用的是一致性的 Hash 算法吗
>是 Hash 槽

