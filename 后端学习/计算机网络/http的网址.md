---
Created: 2024-01-17
Updated: 2024-01-17
Type: knowledge
Status: 🎃已完成
截止日期: 
目标: 
领域: 
tags:
---


> 要搞懂HTTP甚至网络应用，就必须搞定URI

[http学习地址](https://learn.lianglianglee.com/%E4%B8%93%E6%A0%8F/%E9%80%8F%E8%A7%86HTTP%E5%8D%8F%E8%AE%AE/11%20%20%E4%BD%A0%E8%83%BD%E5%86%99%E5%87%BA%E6%AD%A3%E7%A1%AE%E7%9A%84%E7%BD%91%E5%9D%80%E5%90%97%EF%BC%9F.md)
## URI的格式

URI本质是一个字符串，用来**唯一的**标记资源的位置或者名字

他实际上不止能标记万维网的资源，也可以表示其他的，如邮件系统、本地文件系统等任意资源。而“资源”既可以是存在磁盘上的静态文本、页面数据，也可以是由 Java、PHP 提供的动态服务。

## URI 的基本组成

![Pasted image 20231230011324](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/Pasted%20image%2020231230011324.png)
scheme 之后，必须是**三个特定的字符**“**://**”，它把 scheme 和后面的部分分离开。

完整形态的URI
![Pasted image 20231230012218](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/Pasted%20image%2020231230012218.png)
‘#’ 表示一个锚点，或者是说标签，浏览器在获取到资源以后直接跳转到他指定的位置，但是片段标识符服务器是看不到的，就是说浏览器是不会把‘#fragment’发送给服务器，也就是说这是客户端的处理

==我理解vue的路由应该也就是这个原理，router实际上就是标记了对应的资源信息==

## URI的编码
URI里面只能用ASCII码，如果要在URI里面使用英语之外的语言，就需要转义
URI 转义的规则有点“简单粗暴”，直接把非 ASCII 码或特殊字符转换成十六进制字节值，然后前面再加上一个“%”。

例如，空格被转义成“%20”，“?”被转义成“%3F”。而中文、日文等则通常使用 UTF-8 编码后再转义，例如“银河”会被转义成“%E9%93%B6%E6%B2%B3”。

## 小结
 1. URI是 ==唯一==标识资源的一个字符串
 2. URI的组成
 3. ‘#’标记是客户端进行操作，不是服务端进行操作
 4. 转义