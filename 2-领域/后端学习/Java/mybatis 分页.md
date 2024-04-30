---
created: 2024-04-30
updated: 2024-04-30
Type:
  - knowledge
Status:
  - ⌛️ 等待
tags: 
description: 刚开始分页，记录一下分页的小问题
---
##  正常原始分页
思路就是直接写 sql，通过传进来的参数进行分页，一共分三步，
- ~ 第一步先写 mapper. xml
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240430190241.png)
其中，这个 id 就是要写到 mapper 中的方法名，我们就可以在方法中调到
这里的参数是要自己提前写好对应的类的
- ~ 第二步写 mapper.java 文件
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240430190459.png)
只要要求 mapper 和 xml 的 id 对应上就行了

- ~ 第三步就是处理逻辑问题，比如这个 offeset 实际上是 `current*size` 的
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240430190648.png)

写一个测试用例
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240430190728.png)


![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240430190715.png)


返回没什么问题，但是这样以来，后面所有的表的分页都十分之麻烦，每个都要自己重写 sql，一旦表结构变化，要重写很多东西，所以，浅尝辄止，换插件的形式，pageHelper

## PageHelper 分页

