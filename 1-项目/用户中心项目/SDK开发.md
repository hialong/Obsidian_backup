---
created: 2024-05-14
updated: 2024-05-14
Type: knowledge
Status: ⌛️ 等待
tags:
---

## 创建 SDK 的目的

希望能够像 springBoot-starter 一样，只要别人引入，就能用最简单的方式比如配置 springboot 里面的 yml 文件进行简单的配置，类似这种 ![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240516105733.png)
就能使用我提供的能力
## accessKey 和 sceretKey

通过这两个值判断用户是否可以调用后端的接口，一般为了保证安全性还需要加一个随机数，nonce 去判断，防止别人走代理啥的给你接口一直重放

## 如何写一个 SDK

首先需要新建一个 springBoot 项目，我是直接选的最新的 springBoot 以及 java21 ，依赖就选 java web 、devtool、loombook，加上最重要的 configuration 这个![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240516174151.png)
前面的其他的看着加，这个依赖必须加

新建项目后，需要删除掉里面的 build 块，和入口方法，因为用不着![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240516174306.png)

然后在原来的入口方法的路径下，创建一个 cofig 类![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240516174346.png)

测试方法也可以删掉了，因为打包的时候测试方法报错，就上面那个绿色测试类里面

然后重点来了
在 resource 目录下，创建 `META-INF` 文件夹，**文件夹名字很重要，不然不生效**，里面创建文件 `spring.factories`  如下![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240516174618.png)
里面的类就是你配置类的地址，这样打包的时候就能找到你的配置类了

然后直接 intsall 就行了

install 完成之后，你的本地 maven 仓库里面就会有这个