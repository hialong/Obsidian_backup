---
created: 2024-05-11
updated: 2024-05-11
Type: knowledge
Status: ⌛️ 等待
tags:
---
##  整合 knifej

springBoot 2 版本的时候用的 springfox 下的 swagger，但是 springboot3 不一样，改了很多，分两点讲

### springBoot 2

首先引入依赖![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240511141239.png)
```java
<!-- https://doc.xiaominfo.com/knife4j/documentation/get_start.html-->  
<dependency>  
<groupId>com.github.xiaoymin</groupId>  
<artifactId>knife4j-spring-boot-starter</artifactId>  
<version>3.0.3</version>  
</dependency>
```

然后创建一个类随便配置一下
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240511141306.png)


### springBoot 3 以上
咱们根据文档可以看到
[快速开始 | Knife4j](https://doc.xiaominfo.com/docs/quick-start)
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240511141405.png)
这个依赖都不一样了，那么配置自然就不一样了
 所以先引入
 
```java
<dependency>  
<groupId>com.github.xiaoymin</groupId>  
<artifactId>knife4j-openapi3-jakarta-spring-boot-starter</artifactId>  
<version>4.4.0</version>  
</dependency>
```

然后配置也不一样了
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240511141510.png)
