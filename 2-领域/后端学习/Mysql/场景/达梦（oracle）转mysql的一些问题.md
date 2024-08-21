---
created: 2024-08-21
updated: 2024-08-21
Type:
  - knowledge
Status:
  - ⌛️ 等待
tags:
---
## 显示插入开启和关闭
数据迁移的时候发现一个问题
达梦（国产 oracle）在插入数据的时候，会有个这个

![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240821134848.png)


当显示插入开启的时候，才能允许数据插入，否则是不行的，

转换到 mysql 就是
```mysql
-- 启用显式插入标识列的值  
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='NO_AUTO_VALUE_ON_ZERO';  
  
-- 插入数据  
INSERT INTO qiz_hd.XXL_JOB_GROUP (ID, APP_NAME, TITLE, ADDRESS_TYPE, ADDRESS_LIST, UPDATE_TIME)  
VALUES (1, 'xxl-job-executor-sample', '示例执行器', 0, NULL, '2022-09-05 11:07:47');  
  
-- 恢复原来的 SQL 模式  
SET SQL_MODE=@OLD_SQL_MODE;
```