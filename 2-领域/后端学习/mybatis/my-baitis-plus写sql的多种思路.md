---
created: 2024-07-19
updated: 2024-07-19
Type: knowledge
Status: ⌛️ 等待
tags:
---
## 常见写法 xml 格式

常见的就是写 mapper 和标签的形式去写 sql，形如下![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240719234713.png)
这种形式常见，标签处理比较符合长期开发 springmvc 时代的习惯

## mybatis-plus 的注释写法

直接 mapper 上面用注释去写 sql，一种是这样的
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240719235001.png)
直接去写 sql 语句

另一种就是这个写法其实兼容了 xml 的写法直接用 `script标签` 进行 sql 的动态拼接
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240719235313.png)
当然不止@select，还有@update 之类的，但是大同小异

## 推荐写法，@xxxprovider

这种写法就是完全的 java 方式写 sql，首先去你的 mapper 类上写上注解
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240719235431.png)
当然还有 updateProvider 等标签，只举一个例子，
上面的注解意思就是，我们找到这个注解里面的类的方法，下面的参数会直接传进去
参数会根据你@param 的注解的名称（或者顺序--意思是你不写注解也能通过传参顺序找到对应的参数）拿到对应的数据，让直接这样用匿名内部类写 sql ![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240719235602.png)
官网如下 [mybatis – MyBatis 3 | SQL 语句构建器](https://mybatis.org/mybatis-3/zh_CN/statement-builders.html)

![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240719235814.png)

### 这样写的好处
1. ~ 第一个就是，这样写的话，非常的易懂，方便，相比于 xml 的写法，这里每一句都能加上注释，写 java 代码习惯的更方便
2. ~ 第二个这样的动态 sql 的创建非常方便，遇到了复杂的动态条件的 sql 完全可以把动态条件写到一个地方，集中查看，比如，我需要动态生成一个数组，用数组做条件，你完全可以在这个 java 类里面实现，集中到一起更方便，容易懂
3. ~ 这个 sql 是 java 类，那么就能使用切面，比如自定义注解做 sql 层面的权限校验，很有效果
4. ~ 多表联查出现慢 sql，这里完全可以调用其他的 mapper，先查别的表，再查新的表，提升 sql 运行效率