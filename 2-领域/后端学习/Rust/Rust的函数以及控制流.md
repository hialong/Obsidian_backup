---
created: 2024-11-09
updated: 2024-11-09
Type: knowledge
Status: ⌛️ 等待
tags:
---
## 函数

   函数的命名规范  
     
![672f6929eba69a000000015d.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/672f6929eba69a000000015d.png)
  
采用下划线替代驼峰  
函数参数  
比较类似java，参数类型必须被指定  
rust是一个基于表达式的语言  
表达式本身是一个值  
函数的定义是一个语句（非表达式，和java不同），没有返回值，比如下面的这样是不行的（好像java也不行）  
![672f698feba69a0000000171.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/672f698feba69a0000000171.png)
  
  
花括号是一个表达式，是一个块  
![672f67bdeba69a000000012d.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/672f67bdeba69a000000012d.png)
  
这个x+3没有分号的话，就是一个表达式，y就是4，但是如果加上了分号，那么里面就是一个语句了，y的类型就变成了一个tuple，一个（）  
  
但是函数可以有有返回值  
![672f684deba69a0000000147.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/672f684deba69a0000000147.png)
  
注意，函数里面的最后一个表达式就是函数的返回值，如果想提前返回，就是用return（注意返回语句不要加分号，否则就是语句了）  
![672f6a4feba69a0000000182.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/672f6a4feba69a0000000182.png)
  
也可以用return 提前return  
![672f6b41eba69a000000019b.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/672f6b41eba69a000000019b.png)
  
控制流  
if 表达式  
![672f6b93eba69a00000001a3.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/672f6b93eba69a00000001a3.png)
  
同样也有if else if  
但如果使用了超过一个的else if 那么代码会比较乱，建议用match  
![672f6c35eba69a00000001be.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/672f6c35eba69a00000001be.png)
  
或者  
![672f6c8ceba69a00000001c8.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/672f6c8ceba69a00000001c8.png)
  
由于if是一个表达式，那么if里面的值是可返回赋值的类似于三元表达式
![672f6cc6eba69a00000001dc.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/672f6cc6eba69a00000001dc.png)
  
Rust 的循环  
Rust提供了三种循环，loop while 和for  
循环也能有返回值？？  
break 后面还能直接接表达式返回  
![672f6dfceba69a00000001f4.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/672f6dfceba69a00000001f4.png)
  
for循环  
![672f6e83eba69a00000001fe.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/672f6e83eba69a00000001fe.png)
  
![672f6eb2eba69a0000000201.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/672f6eb2eba69a0000000201.png)
  
      
![672f6fb3eba69a0000000215.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/672f6fb3eba69a0000000215.png)
  
注意是小于，不是小于等于哦