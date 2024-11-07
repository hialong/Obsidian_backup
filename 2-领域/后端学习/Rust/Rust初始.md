---
created: 2024-11-04
updated: 2024-11-04
Type: knowledge
Status: ⌛️ 等待
tags:
---
##  关于变量


入门hello world  
    1.  cargo run 编译运行  
    2. cargo build 打包  
        1. 如果正常打包测试会生成在debug目录下面  
        2. 如果加后缀，cargo build --release 则会编译优化，编译时间更长，代码运行更快，且生成在/target/release 目录下面  
    3. cargo check 检查代码，确保能通过编译，反复调用cargo check  
    4. mut 声明变量是可变的，&取地址符号，引用是默认不可变的，也要加上mut才能可变 ![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20241104001108.png) 如果需要使用指针则使用&去操作![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20241104001158.png)
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20241104001205.png)

## 关于包管理

类似于 pom。xml 和 package.json

![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20241104001249.png)
rust 的包管理更类似于前端，这里是包的版本和名字
另外还有 lock 文件用来控制版本

![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20241104001400.png)


如果想要找一些库似乎也可以用 rr 里面的工具下载对应的包