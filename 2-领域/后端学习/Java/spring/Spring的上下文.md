---
created: 2024-07-31
updated: 2024-07-31
Type: knowledge
Status: ⌛️ 等待
tags:
---
## 切入点

一些没有被 Spring 管理的类如何获取其自动注入的 bean？
我使用了这个方式
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240731115649.png)

然后在对应的启动类要 set 进去
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240731115712.png)


另外还有一种方式，但是感觉比这个复杂[解决Springboot中继承ApplicationContextAware获取到的applicationContext为null\_applicationcontextaware 中 setapplicationcontext 为n-CSDN博客](https://blog.csdn.net/sinat_39314995/article/details/107411162)

需要实现# ApplicationContextAware，还需要显示声明 bean 才行