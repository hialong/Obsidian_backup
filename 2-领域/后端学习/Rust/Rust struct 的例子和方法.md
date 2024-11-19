---
created: 2024-11-20
updated: 2024-11-20
Type: knowledge
Status: ⌛️ 等待
tags:
---
## 例子

参数放引用就是借用，借用的方法无法修改借用的参数  
![673cb094eba69a000000009f.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/673cb094eba69a000000009f.png)
  
![673cb0d4eba69a00000000a9.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/673cb0d4eba69a00000000a9.png)
  
rust 中println这个宏，里面的花括号 这个实际上是调用了Display 这个方法，有点类似toString？如果你的结构体没有实现这个方法，就会报错  
  
  
如果灭有实现display，那么可用debug这个方法，即在struct前面显式的加上注解 然后用{：？}或者{：#？}的方式打印（后者的打印更好读）  
![673cb1dceba69a00000000dc.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/673cb1dceba69a00000000dc.png)
  
strct 定义方法  
需要使用到impl关键字  
![673cb317eba69a0000000103.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/673cb317eba69a0000000103.png)
  
这里的方法中有self，是借用，但是也可以获得其所有权，或者获得其借用的所有权  
  
## 方法调用的运算符  
C/C++ 有两种  
一种是 object->something() 的指针指向方法（指针的解引用，就是从指针指向的地址找到对应的值，**注意是值而不是引用地址**）  
一种是调用对象上面的方法，需要解引用后用点去调用方法  
![673cb45ceba69a000000012d.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/673cb45ceba69a000000012d.png)
  
但是Rust里面没有箭头指向运算符，Rust会自动解引用  
  
方法可以有多个参数  
![673cb57aeba69a0000000159.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/673cb57aeba69a0000000159.png)
  
  
  
关联函数  
![673cb59feba69a0000000168.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/673cb59feba69a0000000168.png)
  
类似于静态方法，用双冒号调用  
![673cb612eba69a0000000176.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/673cb612eba69a0000000176.png)
  
每个struct允许拥有多个impl块，可以匹配多个