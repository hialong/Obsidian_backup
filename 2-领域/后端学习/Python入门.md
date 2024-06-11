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

![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240611013202.png)



### 条件判断

计算机之所以能做很多自动化的任务，因为它可以自己做条件判断。

比如，输入用户年龄，根据年龄打印不同的内容，在Python程序中，用`if`语句实现：

```
age = 20
if age >= 18:
    print('your age is', age)
    print('adult')
```

根据Python的缩进规则，如果`if`语句判断是`True`，就把缩进的两行print语句执行了，否则，什么也不做。

也可以给`if`添加一个`else`语句，意思是，如果`if`判断是`False`，不要执行`if`的内容，去把`else`执行了：

```
age = 3
if age >= 18:
    print('your age is', age)
    print('adult')
else:
    print('your age is', age)
    print('teenager')
```

注意不要少写了冒号`:`。

当然上面的判断是很粗略的，完全可以用`elif`做更细致的判断：

```
age = 3
if age >= 18:
    print('adult')
elif age >= 6:
    print('teenager')
else:
    print('kid')
```

`elif`是`else if`的缩写，完全可以有多个`elif`，所以`if`语句的完整形式就是：

```
if <条件判断1>:
    <执行1>
elif <条件判断2>:
    <执行2>
elif <条件判断3>:
    <执行3>
else:
    <执行4>
```

`if`语句执行有个特点，它是从上往下判断，如果在某个判断上是`True`，把该判断对应的语句执行后，就忽略掉剩下的`elif`和`else`，所以，请测试并解释为什么下面的程序打印的是`teenager`：

```
age = 20
if age >= 6:
    print('teenager')
elif age >= 18:
    print('adult')
else:
    print('kid')
```

`if`判断条件还可以简写，比如写：

```
if x:
    print('True')
```

只要`x`是非零数值、非空字符串、非空list等，就判断为`True`，否则为`False`。

### 再议 input

最后看一个有问题的条件判断。很多同学会用`input()`读取用户的输入，这样可以自己输入，程序运行得更有意思：

```
birth = input('birth: ')
if birth < 2000:
    print('00前')
else:
    print('00后')
```

输入`1982`，结果报错：

```
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unorderable types: str() > int()
```

这是因为`input()`返回的数据类型是`str`，`str`不能直接和整数比较，必须先把`str`转换成整数。Python提供了`int()`函数来完成这件事情：

```
s = input('birth: ')
birth = int(s)
if birth < 2000:
    print('00前')
else:
    print('00后')
```

再次运行，就可以得到正确地结果。但是，如果输入`abc`呢？又会得到一个错误信息：

```
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: invalid literal for int() with base 10: 'abc'
```

原来`int()`函数发现一个字符串并不是合法的数字时就会报错，程序就退出了。

如何检查并捕获程序运行期的错误呢？后面的错误和调试会讲到。

### 练习

小明身高1.75，体重80.5kg。请根据BMI公式（体重除以身高的平方）帮小明计算他的BMI指数，并根据BMI指数：

- 低于18.5：过轻
- 18.5-25：正常
- 25-28：过重
- 28-32：肥胖
- 高于32：严重肥胖

用`if-elif`判断并打印结果：

# -*- coding: utf-8 -*-

height = 1.75
weight = 80.5 