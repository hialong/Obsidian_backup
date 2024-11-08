---
Created: 2024-01-17
Updated: 2024-01-17
Type: knowledge
Status: 🎃已完成
截止日期: 
目标: 
领域: 
tags:
---
我们可以通过 `slaveof` 来设置主从，但是 Slave 和 Master 的数据是如何同步的呢？我们从流程一步步看

首先是成为 Slave 节点
	通过 `laveof` 命令或者 `replicateof` (两个命令是一样的，只是改了名)，就能成为 Master 的 Slave, 成功之后，Slave 会记录下 Master 的 IP 和端口

建立连接
	1. Slave 每隔一秒都会检测一下，要不要和 Master 建立连接
	2. 创建 Socket 连接
```C
/* Check if we should connect to a MASTER*/
if (server.repl_state == REPL_STATE_cONNECT）{
	serverLog(LL_NoTICE,"Connecting to MASTER%s:%d",
		server.masterhost, server.masterport);
	if (connectWithMaster() == C_OK) {
		serverLog(LL_NoTICE,"MASTER <-> REPLICA Sync started");
	}
}
```

在上面的代码中的 connectWithMaster 中，如果连接成功，就会为 Socket 建立一个专门处理复制事项的文件事件处理器，来负责后续的复制工作。对于 Master 而言，接收到从节点的 socket 连接后，会为该 socket 创建相应的客户端记录，也就是说，将从节点看作一个普通的客户端，后面的复制其实是以客户端-服务器请求回复的模式来进行。

建立连接完成之后，就进入数据同步状态 Slave 会主动向 Master 发送 psync 命令，首次新添加的 Slave 会触发全量复制，其余时候是增量复制
	**增量复制**即将缓冲区中记录的操作发给 Slave，Slave 复刻这些操作进行同步。
	**全量复制**是说 Master 先生成一个类似 RDB 文件的内存数据快照，然后将快照发给 Slave，Slave 直接将快照加载到内存。
