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
	
## Redis String 

- 是什么：字符串
- 场景：Json，二进制，数字字符串
- 限制，最大 512 M

- 怎么用：
  - 创建：set  setnx (数据已经存在就不会覆盖)

  - 查询: get mget（获取一批）

  - 更新: set

  - 删除: del 

  - Set  strniuniu ex 10 (设置 string 的同时加上过期时间)

    ![image-20231217210935567](D:\study\img\image-20231217210935567.png)

 ### 一些操作的例子
 
String 的操作几乎是最常用的了，所以必须要掌握，多写几个例子吧
首先就是最重要的 Set 命令

String 的创建和更新都用到 Set
```Bash
127.0.0.1：6379 > SET straaa cat
ok
```

除了上面的简单写法，Set 命令还有扩展，
	1. EX second: 设置键值的过期时间为多少秒
	2. PX millisecond: 设置键值过期时间为多少毫秒
	3. NX: 只在键不存在的时候才对键进行设置操作，`SET key value NX` 效果等同于 SETNX key value 😀
	4. XX: 只在键值存在时候，才对键值进行设置操作 ^244caf

下面是一些简单的代码展示
```bash
127.0.0.1：6379 >SET straaa fish NX
null
127.0.0.1：6379 >SET straa fish 
OK
127.0.0.1:6379> set straaa fish
OK
127.0.0.1:6379> set straaa cat nx
(nil)
127.0.0.1:6379> setnx straa cat
(integer) 1
127.0.0.1:6379> setnx straa cat
(integer)0
127.0.0.1:6379>
```
## String 的底层编码

​	string 对象编码有三种

	> redis字符串对象是由redisObject和sdshdr两部分组成的

- INT

    数字之类的用 INT，占空间小（只针对整形）

- EMBSTR

  字符串长度比较小的时候用这个编码，有点是，可以一次性分配内存，字节头和内容是在一起的

  > 缺点在于，如果重新分配空间，整体都需要再分配，所以 EMBSTR 被设置为只读，任何写操作以后，EMBSTR 都会变成 RAW 格式，因为写操作的字符都会被认为是易变的，易变的就用 raw 更方便更新

- RAW

  sds和RedisObject是分开的，使用在长的字符串上，字节头和内容是分开的（有个阈值，超过阈值就用这个来存储字符串，目前比较新版本是 44（5.0.5））

![image-20231217211908559](C:\Users\92502\AppData\Roaming\Typora\typora-user-images\image-20231217211908559.png)

### 	关于 sdshdr

​	![image-20231217212109893](D:\\study\img\image-20231217212109893.png)
另外要注意的是，Redis 中的 SDS 分为 sdshdr 8, sdshdr 16, sdshdr 32, sdshdr 64, 字段属性一样，但是对应不同大小的字符串, 以 sdshdr8 为例，**这里的 8 代表可以用 8 个 bite 记录字符串长度** 8 位可以表示的数字范围是 ±2 的 8 次方（包括负数）然后一个 char 是8比特大小，即一个字节，sdshar8表示的是 char 的长度，也就是说 embstr 只能用 sdshdr 8 (最合适是的，也有特殊的 sdshdr 5 啥的，就是特殊情况了)
```C
// from Redis 7.0.8
struct __attribute__ ((__packed__)) sdshdr8{
    unit8_t len;/*used*/
    unit8_t alloc;/*excluding the header and null terminator*/
    unsigned char flags; /* 3 Lsb of type, 5 unused bits*/
    char buf[]
};
```
### 一些面试题

##### 浮点型

​	浮点型在 redis 中怎么表示的呢，因为 int 只针对整形，所以，浮点型会转换为对应的字符串，比如 3.14-->"3.14"，然后根据长短（阈值 44（5.0.5）），用 embstr 或者 raw，long 以内的整数就以 INT 编码存储

##### String 大小

​	一个 redis 字符串最大为 512 MB，5.0.5 里面源码也是直接写死的

##### SDSHDR 有什么用

1. Redis 是 c 语言写的，sds 就是对 C 语言字符串的封装，自带返回字符串长度，而不需要 c 语言一样遍历才能得到长度，
2.  而且因为自带长度，所以不需要再以 /0 作为结束判断，二进制安全，
3. 有预留空间方便扩展，节约性能

​		

### String 总结

1. 基础操作部分，get  set  setnx，ex 加过期时间（==set 会覆盖或者擦除过期时间==）
2. 字符串结构
3. 场景：缓存
4. 编码格式：int, embstr, raw（embstr 的只读）
5. SDSHDR：raw 的数据指针、优点、预留空间