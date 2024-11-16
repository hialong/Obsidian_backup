---
created: 2024-11-16
updated: 2024-11-16
Type: knowledge
Status: ⌛️ 等待
tags:
---
## struct 简介
**struct  
定义一个struct 一个结构体  
类似java中的对象  
![6738b9c8eba69a0000000122.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/6738b9c8eba69a0000000122.png)
  
需要注意的点是，即使是最后一组对象的Field，后面也要有逗号  
  
  
声明一个struct后，可以用下面的方式构建一个实例  
![](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/6738bbb5eba69a0000000147.png)  
一旦struct 是可变的，那么实例中所有的字段都是可变的  
struct可以作为函数返回值  
初始化struct可以简写  
![6738bc27eba69a0000000162.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/6738bc27eba69a0000000162.png)
  
名称一模一样可以去掉后面的冒号  
## struct 更新语法  
当你想基于某个struct创建一个新实例的时候，可以使用struct的更新语法  
 一般情况下，如果你想基于某个struct实例创建一个新的实例的时候，可以这么写（非更新语法）  
![6738bcc3eba69a00000001a1.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/6738bcc3eba69a00000001a1.png)
  
这里是传统的写法，更新的语法就是  
![6738bd59eba69a00000001b2.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/6738bd59eba69a00000001b2.png)
  
## Tuple struct  
适用于想给整个tuple起名字，并让它不同于其他tuple，又不需要给每个元素取名，  
![6738be24eba69a00000001d2.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/6738be24eba69a00000001d2.png)
  
## 空结构体，Unit_like Struct 没有任何字段  
  
这里比较复杂，可以想象想要实现某个接口，但是里面没有想要储存的数据  
![6738bed9eba69a00000001f3.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/6738bed9eba69a00000001f3.png)
  
![image](6738bf00eba69a00000001fc/6738bf00eba69a00000001fb.png)  
struct 的数据所有权，  
     1. .简单来说就是实例用的是原始值不是引用，那么只要struct 是有效的，那么里面的字段就是有效的，  
    2. 也可以使用引用，但是这需要使用生命周期  
        1. 生命周期保证了只要struct 实例是有效的，那么里面的引用也是有效的**