---
created: 2024-07-09
updated: 2024-07-09
Type: knowledge
Status: ⌛️ 等待
tags:
---
## 解决方法

### 直接打包
将包源码拿出来放到别的项目里面，然后直接本地 install 一下，打包到本地仓库里面

### 直接删除 remote-repository
找到自己的本地仓库找到源码，然后删除这个文件，maven 打包就不会去远程仓库找了
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240709135108.png)

![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240709135238.png)
