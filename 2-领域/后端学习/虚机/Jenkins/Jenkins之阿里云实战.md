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

### 安装 java 环境

直接一键安装
（如果没有 yum 可以看看 agpt 啥的怎么安装的，反正安装了就 ok）
```shell
yum install java-1.8.0-openjdk* -y

## 卸载java
rpm -qa | grep java | xargs rpm -e --nodeps
```

或者官网安装 jdk参考文档 [【JVM系列】安装JDK 21 - 掘金](https://juejin.cn/post/7316202808984780827)
这里我需要安装 jdk-21，所以学习一下

### 官网方式安装 jdk 21
先去官网[JDK Builds from Oracle](https://jdk.java.net/)
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240801175224.png)

点击下载页面，然后找到![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240801175502.png)

建议用 edge 下载，然后暂停，右键复制下载链接
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240801180844.png)
`https://download.oracle.com/java/21/latest/jdk-21_linux-x64_bin.tar.gz`

然后直接虚拟机里面直接下
```shell
wget https://download.oracle.com/java/21/latest/jdk-21_linux-x64_bin.tar.gz
```

下好了解压
```shell
tar -xzvf jdk-21_linux-x64_bin.tar.gz
```

随便把 jdk 移动到一个地方
```shell
mv jdk-21.0.4 /opt/
```

**写入环境变量**


```shell

vim ~/.bashrc

#在后面加上这个
export JAVA_HOME=/opt/jdk-21.0.4/
export PATH=$JAVA_HOME/bin:$PATH

# 然后运行这个，生效环境变量
source ~/.bashrc
```

成功
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240801230158.png)

### maven

下载，太慢了直接下文件也行的
```shell
wget https://dlcdn.apache.org/maven/maven-3/3.9.8/binaries/apache-maven-3.9.8-bin.tar.gz
```
还是 vim ~/. bashrc

然后写个整体的![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240801232910.png)

环境变量冒号隔开
然后 `source ~/.bashrc`
运行这个命令让环境变量生效就行了


然后修改一下 maven 地址还有仓库啥的就行，找到里面的 conf 的 setting. xml
主要是配置镜像。其他的没什么了

![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240801234853.png)



## git
直接 yum install git 就行了
`git --version ` 查看