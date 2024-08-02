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
