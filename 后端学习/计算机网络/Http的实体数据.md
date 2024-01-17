---
Created: 2024-01-17
Updated: 2024-01-17
Type: knowledge
Status: 🌱 完成
截止日期: 
目标: 
领域: 
tags:
---
在 TCP/IP 协议里，传输数据基本上是 header +body 的格式，但是由于其是[[网络部分基础#^a8ddaf|传输层]] 的协议，他不会关心 body 里面的数据是什么
> 关于协议部分详见 [[网络部分基础#模型中每一层的作用]] 目前的广泛的是 TCP/IP 模型的五层

但是 HTTP 协议则不同，他是应用层的协议，数据传输到达以后，HTTP 就要负责告诉上层（他的上层是传输层）这到底是什么数据，这里就要引出 HHTP 是如何标记 body 的数据类型的了，叫做 [[#MIME type]]

### MIME type

MimeType 把数据分成了八大类，每个大类下再细分出多个子类，形式是“type/subtype”的字符串，常见的有
	1. Text：即文本格式的可读数据，我们最熟悉的应该就是 text/html 了，表示超文本文档，此外还有纯文本 text/plain、样式表 text/css 等。
	2. Image：即图像文件，有 image/gif、image/jpeg、image/png 等。
	3. Audio/video：音频和视频数据，例如 audio/mpeg、video/mp 4 等。
	4. Application：数据格式不固定，可能是文本也可能是二进制，必须由上层应用程序来解释。常见有 application/json，application/javascript、application/pdf 等，另外，如果实在是不知道数据是什么类型，像刚才说的“黑盒”，就会是 application/octet-stream，即不透明的二进制数据。

但是 mime type 是不够的，有时候，http 为了节约带宽，还会压缩数据，所以我们有时候还需要另一个解读的方式就是 ==Encoding type==，告诉数据是怎么解压的，常见的有
	1. Gzip：GNU zip 压缩格式，也是互联网上最流行的压缩格式；
	2. Deflate：zlib（deflate）压缩格式，流行程度仅次于 gzip；
	3. Br：一种专门为 HTTP 优化的新压缩算法（Brotli）。

### 数据类型使用的头字段

此外，HTTP 协议还定义了两个 Accept 请求头字段和两个 Content 字段，用于客户端和服务器进行“**内容协商**”。也就是说，客户端用 Accept 头告诉服务器希望接收什么样的数据，而服务器用 Content 头告诉客户端实际发送了什么样的数据。

![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240108223955.png)
> 客户端希望返回的数据....... 以及服务器希望返回的数据 text/html encoding 是 gzip

就比如我工作中就有遇到过，为了实现 tomcat 的过滤，需要把 vue 页面的 index. Html 变成 index. Vhtml 那这个时候浏览器就认不得了（实际上搜狐浏览器能通过 html 文件头判断出来他是 html，但是 chrome 不行）就要在返回的请求头上加上 Content-type 也是通过 tomcat 的 web. Xml 做到的

### 语言类型与编码

实际上这个就是国际化的问题
所谓的“**语言类型**”就是人类使用的自然语言，例如英语、汉语、日语等，而这些自然语言可能还有下属的地区性方言，所以在需要明确区分的时候也要使用“type-subtype”的形式，不过这里的格式与数据类型不同，**分隔符不是“/”，而是“-”**。

举几个例子：en 表示任意的英语，en-US 表示美式英语，en-GB 表示英式英语，而 zh-CN 就表示我们最常使用的汉语。

关于自然语言的计算机处理还有一个更麻烦的东西叫做“字符集”。

在计算机发展的早期，各个国家和地区的人们“各自为政”，发明了许多字符编码方式来处理文字，比如英语世界用的 ASCII、汉语世界用的 GBK、BIG 5，日语世界用的 Shift_JIS 等。同样的一段文字，用一种编码显示正常，换另一种编码后可能就会变得一团糟。

所以后来就出现了 Unicode 和 UTF-8，把世界上所有的语言都容纳在一种编码方案里，UTF-8 也成为了互联网上的标准字符集。
下面是一些例子

```HTTP
Accept-Language: zh-CN, zh, en
```

```HTTP
Accept-Charset: gbk, utf-8 
Content-Type: text/html; charset=utf-8
```

### 内容协商的质量值 q

Q 的作用就是设置权重，来设置优先级
权重的最大值是 1，最小值是 0.01，默认值是 1，如果值是 0 就表示拒绝。具体的形式是在数据类型或语言代码后面加一个“;”，然后是“q=value”。

比如
```HTTP
Accept: text/html,application/xml;q=0.9,*/*;q=0.8
```
它表示浏览器最希望使用的是 HTML 文件，权重是 1，其次是 XML 文件，权重是 0.9，最后是任意数据类型，权重是 0.8。服务器收到请求头后，就会计算权重，再根据自己的实际情况优先输出 HTML 或者 XML。
==这里要注意的是== 在 HTTP 中 ";"  号的意义是要小于 “,” 的

#### 内容协商的结果

内容协商的过程和算法是不透明的，每个服务器都不一样，但是有时候，服务器会在==响应头==里面加上一个 Vary 字段用于记录协商的过程

```HTTP
Vary: Accept-Encoding,User-Agent,Accept
```
这个 Vary 字段表示服务器依据了 Accept-Encoding、User-Agent 和 Accept 这三个头字段，然后决定了发回的响应报文。

### 小结

1. 数据类型表示实体数据的内容是什么，使用的是 MIME type，相关的头字段是 Accept 和 Content-Type；
2. 数据编码表示实体数据的压缩方式，相关的头字段是 Accept-Encoding 和 Content-Encoding；
3. 语言类型表示实体数据的自然语言，相关的头字段是 Accept-Language 和 Content-Language；
4. 字符集表示实体数据的编码方式，相关的头字段是 Accept-Charset 和 Content-Type；
5. 客户端需要在请求头里使用 Accept 等头字段与服务器进行“内容协商”，要求服务器返回最合适的数据；
6. Accept 等头字段可以用“,”顺序列出多个可能的选项，还可以用“; q=”参数来精确指定权重。