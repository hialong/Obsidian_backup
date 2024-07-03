---
created: 2024-07-03
updated: 2024-07-03
Type: knowledge
Status: ⌛️ 等待
tags:
---
## 查看端口号

打开 poershell

```shell
netstat -ano | findstr :37114
```

通过上面的端口号查找到对应的进程![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240703140302.png)

这里在 netstat 输出中，12268 是进程 ID（PID）。这个数字对应着正在监听 `[: :]: 37114 ` 端口的进程。[::]: 37114 表示 IPv 6 的任意地址（相当于 IPv 4 的 0.0.0.0）上的 37114 端口，`[: : ]: 0 ` 表示等待连接到任意 IPv 6 地址的任意端口。


想看详细信息的话用这个命令

![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240703140731.png)
```shell
 tasklist /v /fi "PID eq 12268"
```
## 通过 pid 找到进程

直接打开任务管理器进入详细信息，通过 pid 找到对应进程，kill 掉
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240703140642.png)


