---
created: 2024-06-24
updated: 2024-06-24
Type: knowledge
Status: ⌛️ 等待
tags:
---
## servlet

spring 启动之前要了解一下 servlet 这个技术
![image.png|735](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240624110938.png)
看源码是这样，重点应该是 service 方法和 init 方法
这个狭义的来说，servlet 只是一个接口，广义的来说，他是一个规范。规定怎么去执行 web 的初始化，调用等

找到进去的 service 的实现类，就能看到是进去了调用了 ![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240624113152.png)
但是在进入这些 servlet 之前，还有一个 dispatch 的方法，进行分发，然后再找到对应的 servlet

找到 servlet 的实现类或者说子类就能找到这个类，就是说实际上这个 dispathcer 间接实现了 servlet 的方法 ![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240624113620.png)


### 调试
调试过程中发现先进的 httpServlet 的 service 方法
- [ ] 未完待续