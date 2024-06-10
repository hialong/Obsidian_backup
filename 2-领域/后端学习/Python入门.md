---
created: 2024-06-09
updated: 2024-06-09
Type: knowledge
Status: ⌛️ 等待
tags:
---
## 工具

pycharm 下载：官网加破解就行

python 环境安装：

## 数据类型

Python 的基本数据类型主要包括以下几种：
1. 整型（int）：表示整数，如 -10、0、17 等。Python 3 中整数没有大小限制。
2. 浮点型（float）：表示小数，如 3.14、-0.001 等。也可以用科学计数法表示，如 3.14 e 2 表示 314。
3. 复数（complex）：由实部和虚部组成，形式为 a + bj，其中 a 和 b 是浮点数，如 3+4 j。
4. 字符串（str）：由一系列字符组成，可以用单引号 ' '、双引号 " " 或三引号 ''' '''、""" """ 来表示，如 'hello'、"world"。
5. 布尔型（bool）：只有两个值，True 和 False，用来表示真或假。
6. 空值（NoneType）：只有一个值 None，表示一个空对象或者缺失的值。
这些基本类型是构建复杂数据结构和进行各种计算的基础。

在 Python 中，通常所说的“数组”实际上指的是“列表”（list）。列表是一种非常灵活的数据结构，可以存储不同类型的元素，并且元素数量是可以改变的。以下是关于 Python 列表的一些基本操作和特点：
- 创建列表:  通过方括号 [] 直接列出元素来创建，元素之间用逗号 , 分隔。
```python
my_list = [1, 2, 3]
```

   - 访问元素：使用索引来访问列表中的元素，索引从 0 开始。
```python
   first_element = my_list[0]  # 访问第一个元素
   
```
- 修改元素：可以通过索引直接赋值来修改列表中的元素。
```python
   my_list[0] = 'a'  # 将第一个元素改为'a'
```


-**添加元素**
1. 使用 append () 方法在列表末尾添加元素。
```python
   my_list.append('new')  # 在列表末尾添加'new'
```
2. 使用 insert (index, element) 在指定位置插入元素。
```python
   my_list.insert(0, 'start')  # 在索引0的位置插入'start'
```

-**删除元素**
1. 使用 remove (value) 删除列表中第一个匹配的元素。
```python
   my_list.remove('new')  # 删除'new'
```
2. 使用 pop ([index]) 删除并返回指定索引的元素，默认为最后一个元素。
```python
   last_element = my_list.pop()  # 删除并返回最后一个元素
```

-**切片**
- 提取子列表，语法为 [start: end]，左闭右开区间。
```python
   sub_list = my_list[1:3]  # 提取第2到第3个元素组成的子列表
   
```

