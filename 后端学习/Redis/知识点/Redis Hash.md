## Redis Hash

- 是什么

  字典，无序
  key field都是String的hash表，储存在Redis中

  他的底层是zipList 或者hashTable都是无序的

- 怎么操作

![image-20231219013243753](D:\\study\img\image-20231219013243753.png)

- hash对象编码

  - zipList（压缩列表）[[Redis List底层以及操作#^2bb21b]] 	
	  - 查询长度的时候，有俩字节的zllen，所以小于65535的时候为O(1),大于65535的时候是遍历的o(n)
	  - 查询某一个key的时候必须要遍历，所以复杂度为o（n） ^3c724e

    如果hash对象保存的所有值和键的长度都小于64字节，并且总的对象元素个数小于512个就用ziplist，否则就是hashTable

	对应到Hash里面的话，实际就是将field-value当作里面的entry放到ziplist里面，如下图，两两一组
	![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240106171015.png) ^ebb717
	  - Hashtable
	#RedisHashTable 
		  - hashTable在无序集合的Set里面也有应用[[Redis Set的操作以及底层#^35d9bd]]
		  - 和Set中的HashTable中不同的是，在Hash里面的hashTable有对应的值的，而set后面始终为null
			  -![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240106171735.png)
	
	