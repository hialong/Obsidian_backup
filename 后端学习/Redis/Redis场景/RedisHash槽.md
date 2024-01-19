---
created: 2024-01-18
updated: 2024-01-18
Type: knowledge
Status: "🎃已完成"
tags:
---
## 哈希槽是什么？
Redis 是用 Hash 槽来管理数据分片的，一个 Redis Cluster 包含 16384 个 Hash 槽，即 2^14 次方，存储在集群中的每一个 key 都能通过关联 Hash 算法关联到某个槽，每个节点负责一部分的槽，查数据也要一一对应

首先看代码定义 ![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240118234129.png) 其中 slot 数组表示某个节点的对应关系，而且这里要**注意的是**，分配 Slot 的过程不是说会让 slot 数组分成若干的部分，而是每个 Redis 都有一整个 slot，这个数组是负责记录对应的槽位是不是自己负责的，如果为 1 表明是自己负责
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240118234502.png)

这是位图，所以可以做到在 o (1) 复杂度的情况下，查到某个槽是不是由该节点负责
- [ ] 可以看看位图的解释
## 储存槽的位置
如果客户端请求错了节点，节点还会友情返回你这个数据应该从哪个节点获得（MOVED）。从这里我们能看出除了记录了自己负责了哪些槽，还需要知道某个槽所在位置。这是怎么实现的呢？首先，在分配槽之后，节点会向集群中其它节点放送消息，告知它负责了哪些槽。![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240118234732.png) 然后节点会维护一个 ClusterState 结构
```C
typedef struct clusterState{
	...
	clusterNode *myself;/*This node*/
	clusterNode *slot[CLUSTER_SLOTS];
	...
}clusterState
```

这里解释一下，这个结构有两个 Node，一个是 Solts ，重点是 slots ,slots 里面存储着一个节点，结构如下，举个例子，`slots[0]` 里面的值是一个节点，这个节点的 key-val 是 0-val，而这个 val 就是对应的储存该槽的 Redis, 所以能很快的查到对应槽的Redis
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240118235254.png)
