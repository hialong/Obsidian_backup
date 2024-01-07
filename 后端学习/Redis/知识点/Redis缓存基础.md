> 由于Redis性能高效，通常可以用作数据库储存的缓存，常见的就有给Mysql的热点数据做缓存的玩法，可以很大程度的提升系统的吞吐量


缓存是什么？
	- 缓存分为两种，服务器缓存和客户端缓存
		- 服务器缓存就是指服务端将数据存入Redis，这样在访问DB之后，可以将从DB得到的数据缓存起来，下次访问就从缓存拿，而不走DB
		- 客户端缓存就是客户端自己把之前储存的结果放在缓存里面，下次再请求时候就能直接拿到结果

## 缓存的几种模式

> - Cache Aside Pattern 旁路缓存模式
>- Read Through Cache Pattern：读穿透模式
>- Write Through Cache Pattern：写穿透模式
>- Write Behind Pattern：又叫Write Back，异步缓存写入模式

### Cache Aside

是什么？
	旁路缓存模式，即最常见的模式，应用的服务会直接把缓存当作数据库的旁路，并==直接和缓存进行交互==，读先读缓存，写先写内存

读流程
	流程如下图
	![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240107224814.png)
	简单来说，优先访问缓存，缓存没有再去查db，然后更新缓存，再返回数据

写流程
	流程如下
	![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240107224939.png)
	写操作的时候，就有点需要注意了，写优先写到数据库里面，然后==直接删除缓存==，那么为什么不更新呢？因为更新相比删除，会更容易出现时序性的问题[[#^0c7604]]

适用场景
	由于写操作的时候直接删除缓存，那么很显然这个更适合于写操作少的场景，读多写少

缺点
	缺陷在于，可能会出现缓存和数据库不一致的情况 [[#缓存和数据库不一致的问题]]
	

#### 缓存和数据库不一致的问题
[先删除缓存还是更新数据库后再删除缓存](https://xiaolincoding.com/redis/architecture/mysql_redis_consistency.html#%E5%85%88%E6%9B%B4%E6%96%B0%E6%95%B0%E6%8D%AE%E5%BA%93-%E8%BF%98%E6%98%AF%E5%85%88%E5%88%A0%E9%99%A4%E7%BC%93%E5%AD%98)

是由于线程并发引起的，
对于Cache Aside 来说，先更新数据库，然后删缓存，可能会有A线程读取完数据库后再将数据写入缓存的过程中，出现了一个写操作的线程，他更新完数据库并删除完了缓存之后，线程A才将查到的旧数据写入缓存，就会出现不一致的现象
但是这个几乎不可能出现，因为缓存的写入比数据库写入快的多，A写回数据的时间远远小于B先写数据库，然后删除缓存的时间
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240107231137.png)

#### 时序性问题

举个例子，比如在多线程的环境，
	thread1更新mysql为5->thread2更新mysql为3->  （但是这个时候，因为时间片的原因，两个线程更新缓存的顺序反过来了） thread2更新缓存为3 ->thread1更新缓存为5，最终正确的数据因为时序性被覆盖了。 ^0c7604


### Write Through

是什么？
	相对于 [[#Cache Aside]] 应用程序需要维护两个数据，一个缓存一个数据库，操作比较麻烦，所以Write Through相当于做了一个封装，封装了一个单独的服务，应用程序就只有一个访问源，服务自身维护自己的访问逻辑
	![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240107231718.png)
	他的特点在于
		- 缓存的及时性更高，写入时候就加载了缓存里面，当然可能会有时序问题
		- 不能忍受数据丢失，和数据不一致，当然Cache Aside也是这样


怎么用？
	这个模式是有配合的，有Write-Through 一般都配合Read-through 一起使用，

使用场景
	一般是银行场景用的多，