---
Created: 2024-01-17
Updated: 2024-01-22
Type:
  - knowledge
Status:
  - 🎃已完成
截止日期: 
目标: 
领域: 
tags:
  - Redis操作
  - Redis跳表
  - 数据结构
---


## Redis跳表

 - 是什么
   	- 有序的多索引链表
 - 场景
   	- ZSET
 - 特点
    - 有序；查询性能高
 - 数据结构
   	
    ![image-20231220001631362.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/image-20231220001631362.png)
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240122230212.png)


查找的话，先查二级索引，超过了要查的索引（比如查8，然后这时候跳到了10，那就跳回去从6往一级索引找），再往下查一级索引，一直定位到要查找的节点

插入和查询类似
### 查询复杂度
跳表的表头结构
```C
typedef struct zskiplist {
    struct zskiplistNode *header, *tail;
    //节点数量
    unsigned long length;
    int level;
} zskiplist;
```
1. 查询节点总数的复杂度
	 看表头的length就可以知道，储存了节点总数，那么复杂度就是O(1) ^b71a01
2. 跳跃列表的平均查找和插入时间复杂度都是O(logn) [[#^39e5b0|插入数据复杂度]]



> 标准的跳表有如下的限制：(Redis不是标准的跳表)
>
> 1. 里面的值（score分数）不能重复  一个节点两个值，一个分数，一个成员值  | redis里面的可以重复，如果分数重复，那么排序就按照成员的字典序进行排序
> 2. 只有向前指针，没有回退指针 | redis里面加了回退指针

### 插入数据复杂度

^39e5b0

先说结论，插入数据平均复杂度是O(logN)，首先是有序集合无论是查找还是增加元素，都要定位到数据位置，所以跳表首先要进行定位，定位的方式跟查询类似，都是上级索引往下找找到下级索引，最坏情况下就是一直没有找到对应的索引，一直到最底层的索引才找到，复杂度是logN  
	这里可以看层数[[#^75fe36]]

### 跳表结构
![image-20231220231836307.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/image-20231220231836307.png)

### 层高确定

^75fe36

 层高的确定是比较随机的，能否增加一层的概率是25%，而Redis5.0最大层数限制是64，7.0是32层
 