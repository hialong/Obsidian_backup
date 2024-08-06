---
created: 2024-08-06
updated: 2024-08-06
Type: knowledge
Status: ⌛️ 等待
tags:
---
## docker 中缺少字体导致 poi 的autoSizeColumn 空指针

需要在 docker 容器中安装字体
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240806142421.png)

但是进去了之后发现没办法下载工具，所以记录一下
alpine 是 linux 的发行版，所以他的下载是 apk add

```shell
apk search openssh  #查询openssh相关的软件包
apk add xxx  #安装一个软件包
apk del xxx  #删除已安装的xxx软件包
apk --help  #获取更多apk包管理的命令参数
apk update   #更新软件包索引文件

```

如果需要换成中国的镜像下载地址
就去这个地址/etc/apk/repositories

修改 repositories 里面的地址，只能 cat 看，由于没有 vim，所以用 echo 去改

```shell
echo 'https://mirrors.aliyun.com/alpine/v3.6/main/' > repositories
echo 'https://mirrors.aliyun.com/alpine/v3.6/community/' >> repositories
```

改完后再下载就快了
