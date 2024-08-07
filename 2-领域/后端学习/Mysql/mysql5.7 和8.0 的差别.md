---
created: 2024-08-01
updated: 2024-08-01
Type: knowledge
Status: ⌛️ 等待
tags:
---
## 性能，5.7>8.0

 MysQL 5.7 里如果不希望写 binlog ，那设置 `log_bin=0` 或者 `log_bin=off` 都可以，而 MySQL 8.0 ,则需要配置为 `skip-log-bin`
这样性能就差不多了

docker 里面的进去/etc/mysql/my. cnf 在 mysqlId 下面添加 skip-log-bin  重启 docker 就行


[默认配置下，为什么 MySQL 8.0 比 MySQL 5.7 慢 - 个人笔记](https://wgzhao.github.io/notes/database/why-mysql8-slower-than-mysql57/#_3)


![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240802101946.png)


## 用命令行导入数据
[MySQL 如何使用命令行导入 SQL 文件？|极客教程](https://geek-docs.com/mysql/mysql-ask-answer/4_mysql_how_do_i_import_an_sql_file_using_the_command_line_in_mysql.html)

将 SQL 文件移动到合适的位置后，在命令行中输入以下命令导入 SQL 文件：

```mysql
mysql -u root -p database_name < file.sql
```

Mysql

Copy

其中 `root` 为用户名，`database_name` 为导入数据的数据库名称，`file.sql` 为要导入的 SQL 文件路径。

## 4. 导入部分 SQL 文件

有时候我们只需要导入 SQL 文件中的某几行数据，这时候可以使用以下命令导入部分 SQL 文件：

```mysql
mysql -u root -p database_name < file.sql --lines=X-Y
```

Mysql

Copy

其中 `X` 为起始行，`Y` 为结束行，表示导入 SQL 文件的第 `X` 行到第 `Y` 行数据。

## 5. 导入已有数据库的表

如果想要将 SQL 文件中的数据导入到已有的数据库和某些表中，则可以使用以下命令导入：

```mysql
mysql -u root -p database_name < file.sql --table=table_name1 --table=table_name2
```

Mysql

Copy

其中 `table_name1` 和 `table_name2` 为已有数据库中的表名。
