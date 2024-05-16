---
created: 2024-05-14
updated: 2024-05-14
Type: knowledge
Status: ⌛️ 等待
tags:
---

## 创建 SDK 的目的

希望能够像 springBoot-starter 一样，只要别人引入，就能用最简单的方式比如配置 springboot 里面的 yml 文件进行简单的配置，类似这种 ![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240516105733.png)
就能使用我提供的加密能力
## accessKey 和 sceretKey

通过这两个值判断用户是否可以调用后端的接口，一般为了保证安全性还需要加一个随机数，nonce 去判断，防止别人走代理啥的给你接口一直重放

## 如何写一个 SDK

