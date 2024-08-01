---
created: 2024-07-30
updated: 2024-07-30
Type: knowledge
Status: ⌛️ 等待
tags:
---
## 下载 jenkins
jekins 超详细 pdf
![[《Devos之Jenkins的CICD》.pdf]]




直接 docker 下载，docker 下载在这里[[项目部署（阿里云）#安装 docker]]
```shell
docker pull jenkins/jenkins
```
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240801093437.png)


然后创建文件夹
```shell
#创建文件夹
mkdir -p /home/jenkins_home
#权限
chmod 777 /home/jenkins_home
```


```shell
docker run -d --name jenkins --restart=always -p 9095:8080 -p 50000:50000  -v /home/jenkins_home:/var/jenkins_home -v /etc/localtime:/etc/localtime jenkins/jenkins
```

这里发现 jekins 的默认端口 8080，可能会跟其他的有冲突 (docker 的话好像不用改，忽略下面这一段，如果不是 docker，请修改后，systemctrl restart jenkins)
```shell
#修改端口号
 vi /usr/lib/firewalld/services/jenkins.xml
#找不到的话，用find找一下
find / -name jenkins.xml
```

然后 web 登录![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240801101301.png)

这时候要下插件的话，最好换源
进入宿主机目录 /home/jenkins_home/，编辑文件 hudson.model.UpdateCenter.xml
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240801101345.png)
将地址修改成 `https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json` 这是清华大学镜像源


修改完记得重启一下容器
docker restart xxxxid

然后刷新一下，直接下推荐的插件（刷新后要重新输入密码，记得看 logs 里面）![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240801101706.png)

创建用户，全是 admin
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240801101900.png)
这个后缀我加了一个路由标签

![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240801102010.png)
**但是好像没啥用**

## 使用 jenkins 打包部署

首先，使用 jekins 打包部署，需要根据条件安装对应的环境
拉取代码需要 **git**
java 的话，需要 **jdk，maven**，
前端代码需要 node 环境

--- 参考 gpt
你可以在宿主机上安装 Java 和 Maven，然后在 Jenkins 的 Docker 容器中使用宿主机的 Java 和 Maven 环境。以下是步骤：  
在宿主机上安装 Java 和 Maven：  
安装 Java：<pre>sudo apt update sudo apt install openjdk-11-jdk </pre>
安装 Maven：<pre>sudo apt install maven </pre>
在 Docker 容器中配置 Jenkins 使用宿主机的 Java 和 Maven：  
启动 Jenkins 容器时，挂载宿主机的 Java 和 Maven 目录到容器中。例如：<pre>docker run -d -p 8080:8080 -p 50000:50000 \ -v /var/run/docker. sock:/var/run/docker. sock \ -v /path/to/host/java:/usr/lib/jvm/java-11-openjdk-amd 64 \ -v /path/to/host/maven:/usr/share/maven \ jenkins/jenkins: lts </pre>
在 Jenkins 中配置 Java 和 Maven 的路径：
进入 Jenkins 的管理界面，点击“Manage Jenkins”。
点击“Global Tool Configuration”。
在“JDK”部分，添加 JDK 并设置路径为/usr/lib/jvm/java-11-openjdk-amd 64。
在“Maven”部分，添加 Maven 并设置路径为/usr/share/maven。
这样，你就可以在 Jenkins 中使用宿主机的 Java 和 Maven 环境了
