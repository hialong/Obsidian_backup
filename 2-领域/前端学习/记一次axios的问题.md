---
created: 2024-07-22
updated: 2024-07-22
Type: knowledge
Status: ⌛️ 等待
tags:
---
## 问题
尝试传参的过程中，尝试传参形如{key, new Map () {{key->value}} }的形式，结果 axios 无法传数据到后端，一直是空
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240722183452.png)

发现 axios 无法传 map 的参数

[blog.51cto.com/u\_16175430/7593067#:\~:text=由于 Axios 目前不直接支持传递 Map 类型的参数，我们需要将 Map 转换为对象，以便能够正常传递参数。,在 JavaScript 中，可以使用 Object.fromEntries %28%29 方法将 Map 转换为对象。](https://blog.51cto.com/u_16175430/7593067#:~:text=%E7%94%B1%E4%BA%8E%20Axios%20%E7%9B%AE%E5%89%8D%E4%B8%8D%E7%9B%B4%E6%8E%A5%E6%94%AF%E6%8C%81%E4%BC%A0%E9%80%92%20Map%20%E7%B1%BB%E5%9E%8B%E7%9A%84%E5%8F%82%E6%95%B0%EF%BC%8C%E6%88%91%E4%BB%AC%E9%9C%80%E8%A6%81%E5%B0%86%20Map%20%E8%BD%AC%E6%8D%A2%E4%B8%BA%E5%AF%B9%E8%B1%A1%EF%BC%8C%E4%BB%A5%E4%BE%BF%E8%83%BD%E5%A4%9F%E6%AD%A3%E5%B8%B8%E4%BC%A0%E9%80%92%E5%8F%82%E6%95%B0%E3%80%82,%E5%9C%A8%20JavaScript%20%E4%B8%AD%EF%BC%8C%E5%8F%AF%E4%BB%A5%E4%BD%BF%E7%94%A8%20Object.fromEntries%20%28%29%20%E6%96%B9%E6%B3%95%E5%B0%86%20Map%20%E8%BD%AC%E6%8D%A2%E4%B8%BA%E5%AF%B9%E8%B1%A1%E3%80%82)


修改如下
![image.png](https://obsidian-pic-1317906728.cos.ap-nanjing.myqcloud.com/obsidian/20240722183553.png)

转换成对象再存到 form 里面就行了