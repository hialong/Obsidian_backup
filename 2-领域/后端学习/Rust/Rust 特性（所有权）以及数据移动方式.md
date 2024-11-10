---
created: 2024-11-10
updated: 2024-11-10
Type: knowledge
Status: ⌛️ 等待
tags:
---
## 所有权
所有权，能**让rust无需gc就能保证内存安全**  
  
所有权  
    1. 所有程序在运行时，都必须管理他们使用计算机内存的方式  
![6730bbe1eba69a000000010b.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/6730bbe1eba69a000000010b.png)
  
stak vs heap  
栈 stack 先进后出  
heap  
![6730bc3ceba69a0000000125.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/6730bc3ceba69a0000000125.png)
  
把值压到stack上不叫分配，指针是固定大小的，可以把指针放在stack上，  
![6730bc71eba69a0000000137.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/6730bc71eba69a0000000137.png)
  
    访问heap中的数据要比stack中要慢的原因在于，需要通过指针才能找到heap中的数据，对于现在的处理器来说，由于缓存的缘故，指令在内存中跳转次数越少，那么速度就越快  
      
      
![6730bd57eba69a000000015b.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/6730bd57eba69a000000015b.png)
  
所有权规则  
    1.  每个值都有一个变量，这个变量是该值的所有者   
    2. 每个值同时只能有一个所有者  
    3. 当所有者超出作用域时，该值将被删除  
  
  作用域：  
    1. 类似其他语言，能被用到的地方就是作用域，比如声明在方法中的变量，在方法结束后就结束作用域  
  
String类型  
可以用from函数，从一个字符串字面值创建一个String变量  
![6730bef6eba69a00000001ab.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/6730bef6eba69a00000001ab.png)
  
这个方式创建的String变量是可修改的，而字符串字面值是不可修改的  
这里涉及到内存的分配问题  
![6730bf7beba69a00000001c3.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/6730bf7beba69a00000001c3.png)
  
  
==但是rust采用了不同的方式==  
当拥有它的变量走出作用范围的时候（方法作用域结束），内存会立即的自动的交还给操作系统  
在走出作用域的时候，会调用一个drop函数  
  
## 变量和数据交互的方式  
    1. Move  
        1. 第一种情况，简单的数据类型？比如  
![6730c0bdeba69a00000001f1.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/6730c0bdeba69a00000001f1.png)
  
这里，x和y是简单的数据类型，创建5的时候，分配给了x，然后又创建了一个x的副本，分配给了y，整数是已知的且固定大小的，所以两个5都在stack中  
    1. 复杂的情况，String这种情况  
        1. 一个String由三个部分组成  
            1. 一个存放字符串内容的内存的指针ptr，指向存放内容的内存  
            2. 一个长度  
            3. 一个容量  
            4.   ![6730c18eeba69a0000000228.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/6730c18eeba69a0000000228.png)
  
上面三个部分放在stack上，而存放字符串内容的部分就在heap上  
类似于java的String  
如果按照一般的做法，如下面的  
![6730c208eba69a0000000235.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/6730c208eba69a0000000235.png)
  
二次释放是一种极其严重的bug ，所以rust采用了一种其他的方式  
![6730c25ceba69a0000000255.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/6730c25ceba69a0000000255.png)
  
如果赋值后再调用之前的s1就会报错  
![6730c2dbeba69a0000000264.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/6730c2dbeba69a0000000264.png)
  
所以就解决了二次释放  
  
![6730c30aeba69a0000000279.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/6730c30aeba69a0000000279.png)
  
rust是性能优先的，但是如果真的想对heap上的数据进行深度拷贝，那么就可以使用clone方法  
![6730c350eba69a0000000288.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/6730c350eba69a0000000288.png)
  
![6730c362eba69a0000000290.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/6730c362eba69a0000000290.png)
  
对于整数类型之类的简单数据类型，能够完全确定自己的大小和空间的，他们的深拷贝和浅拷贝不是很影响，都是复制存到stack上  
![6730c40aeba69a00000002ad.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/6730c40aeba69a00000002ad.png)
  
![6730c452eba69a00000002b0.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/6730c452eba69a00000002b0.png)
  
所有权与函数  
当函数拿到了String类型后，那么x的作用域就走到了函数中了，走完函数后，就会调用drop函数，销毁变量x 所以这里会报错  
![6730c541eba69a00000002c7.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/6730c541eba69a00000002c7.png)
  
但是简单数据类型就不会，因为传到函数的实际上是一个副本  
![6730c58ceba69a00000002cf.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/6730c58ceba69a00000002cf.png)
  
那么如何让函数使用某个值，但是不获得其所有权呢  
那就是引用！！  
引用和借用  
![6730c845eba69a00000002fb.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/6730c845eba69a00000002fb.png)
  
引用就类似于指针，但是引用在离开其作用域的时候，并不会销毁其指向的内存，因为他并没有其所有权  
以引用作为函数参数的这个行为，叫**借用**  
那么我们可以修改借用的东西吗?  
    答案是不行，和变量一样，引用默认也是不可变的  
    但是！如果你在前面加上mut关键字，那么就可以可变了  
![6730c98eeba69a0000000339.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/6730c98eeba69a0000000339.png)
  
但是要注意，对于特定的作用域内  
某一块数据，只能有一个可变的引用

![6730cbe4eba69a00000003a0.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/6730cbe4eba69a00000003a0.png)
  
这样的好处是可在编译时候防止数据竞争  
![6730cc1ceba69a00000003b0.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/6730cc1ceba69a00000003b0.png)
  
那么我们可以通过创建新的作用域，来允许非同时的创建多个可变引用  
![6730cc56eba69a00000003bd.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/6730cc56eba69a00000003bd.png)
  
另外还有一个限制，不可以同时拥有一个可变引用和一个不可变引用，但是多个不可变的引用是可以的  
![6730cd72eba69a00000003e1.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/6730cd72eba69a00000003e1.png)
  
悬空引用  
![6730cd8deba69a00000003e9.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/6730cd8deba69a00000003e9.png)
  
那么我们可以通过创建新的作用域，来允许非同时的创建多个可变引用  
这样的代码是会报错的，因为这样返回的一个指针将会指向一个已经悬空的地址，所以如果想拿到引用，那么引用就必须一直有效