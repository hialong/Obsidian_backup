---
created: 2024-07-30
updated: 2024-07-30
Type: knowledge
Status: ⌛️ 等待
tags:
---
## 参考
[docker搭建nexus3(docker私服+npm私服)\_nexus3 need auth you need to authorize this machin-CSDN博客](https://blog.csdn.net/youlinhuanyan/article/details/121331277)
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240730163506.png)



## 阿里云实验

先安装 docker，如果有问题看这个 [[项目部署（阿里云）#安装 docker]]

然后直接拉镜像
```shell
docker pull sonatype/nexus3

##创建目录
mkdir /docker/nexus3
chmod 777 /docker/nexus3

```

启动的时候注意，不要占用 8081 等端口，否则阿里云直接卡死
```shell
docker run -d --name nexus3 \
--restart=always \
-p 8881:8881 \
-p 8882:8882 \
-p 8883:8883 \
-p 8884:8884 \
-p 8885:8885 \
-v /docker/nexus3:/nexus-data \
sonatype/nexus3

```