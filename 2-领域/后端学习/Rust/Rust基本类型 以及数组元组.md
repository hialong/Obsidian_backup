---
created: 2024-11-07
updated: 2024-11-07
Type: knowledge
Status: ⌛️ 等待
tags:
---
## 变量和基本类型
   1. 变量和可变性  
        1. 默认情况下，变量是不可变的  
        2. 声明变量时候加mut可以使得变量可变  
        3. 常量  
![image](672c469569d71d00000000fd/672c469569d71d00000000fc.png)  
shadowing 新声明变量可以重新声明原来存在的变量，可以隐藏之前的变量，新声明的变量类型可以跟之前的不同  
标量和复合裂类型  
整数类型  
![image](672cdc95eba69a00000000ae/672cdc95eba69a00000000ad.png)  
i表示有符号，U表示无符号的，比如i8就是的到正的  
![image](672cdcd2eba69a00000000b8/672cdcd2eba69a00000000b7.png)  
![image](672cdce7eba69a00000000c0/672cdce7eba69a00000000bf.png)  
![image](672cdd0aeba69a00000000c3/672cdd0aeba69a00000000c2.png)  
整数溢出，rust的环绕操作  
浮点类型f32 f64双精度，f64作为默认的  
  
布尔bool  
字符类型，单引号  
![image](672cddc8eba69a00000000f4/672cddc8eba69a00000000f3.png)  
  
  
复合类型  
元组Tuple 长度固定，一旦声明无法修改，Tuple 中的各个元素类型不必相同  
![image](672cdeaeeba69a0000000108/672cdeaeeba69a0000000107.png)  
解构tuple值类似于js语法  
![image](672cdf02eba69a0000000117/672cdf02eba69a0000000116.png)  
用点方法点index可以找到里面对应的值  
![image](672cdf57eba69a0000000126/672cdf57eba69a0000000125.png)  
数组，里面的数组类型是一致的，长度也是固定的，不能随意修改  
![image](672cdf98eba69a000000013a/672cdf98eba69a0000000139.png)  
如果你不知道应该使用数组还是vector，那么估计你应该使用vector   
![image](672ce002eba69a0000000155/672ce002eba69a0000000154.png)  
用索引访问数组元素  
以及咩办法再骗过编译器了  
![image](672ce12aeba69a000000016d/672ce12aeba69a000000016c.png)