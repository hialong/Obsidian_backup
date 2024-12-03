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

## 绑定值的模式
