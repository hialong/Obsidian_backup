---
created: 2024-08-08
updated: 2024-08-08
Type: knowledge
Status: ⌛️ 等待
tags:
---
## 查看问题

通过show processlist可以看到TableA上有正在进行的操作（包括读），此时alter table语句无法获取到metadata 独占锁，会进行等待。

![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240808143415.png)
kill 加 id 删除被锁进程