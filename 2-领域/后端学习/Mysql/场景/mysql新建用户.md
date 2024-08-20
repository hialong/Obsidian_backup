---
created: 2024-08-20
updated: 2024-08-20
Type: knowledge
Status: ⌛️ 等待
tags:
---
## mysql 用户

```mysql
# 允许本地访问
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '您的密码';
# 允许外网访问
CREATE USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '您的密码';
# 设置允许访问的数据库
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
# 刷新缓存
FLUSH PRIVILEGES;

```