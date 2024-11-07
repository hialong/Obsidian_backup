---
created: 2024-11-07
updated: 2024-11-07
Type: knowledge
Status: ⌛️ 等待
tags:
---
## 枚举类型与异常判断

枚举类型  
    Ordering  
    数字类型中包含方法cmp 用来比较值，使用match表达式可以使得值之间进行比较 返回就是Ordering枚举类  
      整数类型 ，i32,u32,i64默认i32  
        1. 可以重复声明相同的变量，新变量声明后，旧变量将被隐藏，常用于需要类型转换的时候  
        2. rust有强化编译的功能，即如果声明普通的数字类型的变量，将被是为i32,如果下面又出现了数字的比较，比如用U32和这个数字比较，根据方法，会把上面申明的数字当作U32 ![672b8f48eba69a00000000ec.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/672b8f48eba69a00000000ec.png)
==异常判断也及其优雅==
从原本的
```rust
let guss:u32 = guss.trim().parse().except("请输入正确的数");
```
到
```
let guss: u32 = match guss.trim().parse(){  
    Ok(num)  => num,  
    Err(_) =>continue,  
};
```

通过拿到的 Result 枚举类进行判断，下划线表示不关注的变量，里面其实是报错的信息，如果需要应该可以打印
## 循环的写法
rust 的循环 loop {}  
![672b90dbeba69a0000000107.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/672b90dbeba69a0000000107.png)


其中 continue 和 break 仍保持着原来的作用

优雅的代码
```rust
use std::cmp::Ordering;  
use std::io;  
use rand::Rng;  
  
fn main() {  
    println!("猜数！！");  
    println!("猜一个数");  
  
    let secret_num = rand::thread_rng().gen_range(1, 101);  
  
    loop {  
        let mut guss = String::new();  
        io::stdin().read_line(&mut guss).expect("无法读取行");  
  
        let guss: u32 = match guss.trim().parse(){  
            Ok(num)  => num,  
            Err(_) =>continue,  
        };  
        // 猜数的注释  
        println!("你猜的数是：{}", guss);  
  
        match guss.cmp(&secret_num) {  
            Ordering::Less => println!("太小了"),  
            Ordering::Greater => println!("太大了"),  
            _ => {  
                println!("你赢了");  
                break;  
            }  
        }  
    }  
}
```