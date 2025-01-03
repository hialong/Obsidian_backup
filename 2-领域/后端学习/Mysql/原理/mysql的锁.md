---
created: 2024-05-31
updated: 2024-05-31
Type: knowledge
Status: ⌛️ 等待
tags:
---
## 全局锁和表锁以及行锁

**根据加锁的范围，MySQL 里面的锁大致可以分成全局锁、表级锁和行锁三类**。

### 全局锁
 Flush tables with read lock (FTWRL)。
这个命令让表变成只读，实际上可以被可重复读替代

### 表级锁
分为两种，一种是**表锁**，一种是**元数据锁**

#### 表锁
**表锁的语法是 lock tables … read/write。**与 FTWRL 类似，可以用 unlock tables 主动释放锁，也可以在客户端断开的时候自动释放

表锁语句会对当前线程产生影响

#### 元数据锁

MDL 锁是系统默认会加的
MDL 不需要显式使用，在访问一个表的时候会被自动加上。MDL 的作用是，保证读写的正确性。
但是有些地方要注意
- 读锁之间不互斥，因此你可以有多个线程同时对一张表增删改查。
- 读写锁之间、写锁之间是互斥的，用来保证变更表结构操作的安全性。因此，如果有两个线程要同时给一个表加字段，其中一个要等另一个执行完才能开始执行。（这里指的是如果你要往里写数据需要**写锁**，但是别的线程已经拿到了**读锁**，两个线程是互斥的，只有别人释放了读锁，线程才能拿到写锁）