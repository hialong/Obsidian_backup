---
created: 2024-06-11
updated: 2024-06-11
Type:
  - knowledge
Status:
  - ⌛️ 等待
tags:
---
## 外连接时 on 和 where 过滤条件区别？

在SQL查询中使用外连接（如LEFT JOIN、RIGHT JOIN、FULL JOIN）时，`ON`和`WHERE`子句都用于筛选数据，但它们的应用上下文、执行时机和作用方式有所不同。

### [](https://open.bigmodel.cn/trialcenter?modelCode=glm-4#on%E6%9D%A1%E4%BB%B6-%E8%BF%9E%E6%8E%A5%E6%9D%A1%E4%BB%B6)ON条件 - 连接条件

`ON`关键字在SQL连接查询中用于指定两个或更多表之间的连接条件。当执行外连接时，`ON`后面跟随的条件用于决定哪些行应该相互匹配，以形成结果集的一部分。

- **对于LEFT JOIN（左连接）**：`ON`条件决定右表中的哪些行与左表匹配。左表中的所有行都会被包括在结果集中，即使右表中没有匹配的行。
- **对于RIGHT JOIN（右连接）**：`ON`条件决定左表中的哪些行与右表匹配。右表中的所有行都会被包括在结果集中，即使左表中没有匹配的行。
- **对于FULL JOIN（全连接）**：`ON`条件决定两个表中的哪些行相互匹配。无论左表还是右表中的行，如果没有匹配的行，都会被包括在结果集中，不匹配的行以`NULL`填充。

### [](https://open.bigmodel.cn/trialcenter?modelCode=glm-4#where%E6%9D%A1%E4%BB%B6-%E6%95%B0%E6%8D%AE%E8%BF%87%E6%BB%A4)WHERE条件 - 数据过滤

`WHERE`子句在SQL查询中用于对数据行进行过滤。在外连接查询中，`WHERE`子句通常用于对连接后的结果集进行筛选。

- `WHERE`子句中的条件是在连接操作之后应用的，即先形成连接结果，再根据`WHERE`条件进行过滤。
- 在外连接中，`WHERE`子句对已经通过`ON`条件连接的行进行过滤。这意味着，如果`WHERE`条件导致某行被排除，那么该行将不会出现在结果集中，即使它是外连接的一部分。

### [](https://open.bigmodel.cn/trialcenter?modelCode=glm-4#%E5%8C%BA%E5%88%AB%E4%B8%8E%E7%BB%93%E5%90%88%E4%BD%BF%E7%94%A8)区别与结合使用

1. **执行顺序**：`ON`条件在连接过程中先执行，用于确定哪些行参与连接；`WHERE`条件在连接操作完成后执行，用于对最终的结果集进行过滤。
2. **作用对象**：`ON`作用于参与连接的各个表的行，以确定它们是否应该被连接；`WHERE`作用于连接后的整个结果集。
3. **对结果集的影响**：在外连接中，`ON`条件不会排除左表（LEFT JOIN）或右表（RIGHT JOIN）中的任何行，但`WHERE`条件可以排除任何行，包括那些原本应该被外连接包括的行。

### [](https://open.bigmodel.cn/trialcenter?modelCode=glm-4#%E5%BA%94%E7%94%A8%E5%9C%BA%E6%99%AF)应用场景

- 当你需要确保左表或右表中的所有行无论如何都要出现在结果集中时，应该使用`ON`条件来定义连接。
- 如果你需要对连接后的结果进行进一步的筛选，或者需要排除某些行，那么应该使用`WHERE`条件。

### [](https://open.bigmodel.cn/trialcenter?modelCode=glm-4#%E6%80%BB%E7%BB%93)总结

在使用外连接时，理解`ON`和`WHERE`条件的区别非常重要。简而言之，`ON`条件用于确定哪些行可以连接，而`WHERE`条件用于在连接操作之后对结果集进行过滤。正确使用这两个条件可以帮助你获得期望的查询结果。