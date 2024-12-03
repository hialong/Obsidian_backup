---
created: 2024-12-03
updated: 2024-12-03
Type: knowledge
Status: ⌛️ 等待
tags:
---
## 控制流运算符

match 的语法类似于 swith，会拿到参数后**依次**跟 mathc 里面的条件进行匹配

```Rust
match coin{
	Coin::penny => 1,
	Coin::Nickel => 5,
	Coin::Dime => 10,
	Coin::Quarter => 25,
}
```

并且能直接使用枚举类，并可以带有返回值的 !![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20241203234358.png)

match 配合枚举还有一个好处，**就是如果你的 match 没有穷举所有的匹配**，那么就会编译不通过，如果不想处理所有的枚举，那么就可以使用下划线 `_` 表示 deefault

## 绑定值的模式
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20241203235111.png)


匹配 Option 枚举这块直接看上一章[[定义枚举#Option 枚举]]

## if  let

match 的匹配非常严谨，但是也有些麻烦，如果想要对枚举类进行单一的匹配判断，只关心其中一条支线的话就可以使用 if let

**if let** 
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20241204000240.png)
相当于y