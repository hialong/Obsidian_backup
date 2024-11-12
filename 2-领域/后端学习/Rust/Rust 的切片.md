---
created: 2024-11-12
updated: 2024-11-12
Type: knowledge
Status: ⌛️ 等待
tags:
---
## 切片

切片：**不持有**所有权的数据类型 slice  因为拿的是引用的地址，所以在传递的时候不会拿到原来数据的所有权，导致数据的生命周期进入其他的周期里面
  
要确保同步性  
![67336dbeeba69a00000000ab.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/67336dbeeba69a00000000ab.png)
  
这里的s clear后会导致index变得毫无意义，  
字符串切片 【开始索引...结束索引】这里的结束索引是不包括的  
  
切片的结构如下面的那个world  
![image](67336ea7eba69a00000000c0/67336ea7eba69a00000000bf.png)  
是直接指向heap中存储的字符值的  
  
这样这下面这部分就会报错  
![image](67336fceeba69a00000000d9/67336fceeba69a00000000d8.png)  
tips 语法糖  
![image](67336ff9eba69a00000000e8/67336ff9eba69a00000000e7.png)  
最前面的索引和最后面的索引可以不写  
以及整个字符串的切片  
![image](6733702deba69a00000000f5/6733702deba69a00000000f4.png)  
注意  
![image](6733708deba69a0000000104/6733708deba69a0000000103.png)  
字符串字面值 实际上就是一个指向特定位置的一个切片，所以是不可变的  
  
![image](6733721ceba69a000000011c/6733721ceba69a000000011b.png)  
例子  
![image](67337240eba69a000000012b/67337240eba69a000000012a.png)  
其他类型也存在切片，比如整数等  
![image](673372fceba69a000000013a/673372fceba69a0000000139.png)