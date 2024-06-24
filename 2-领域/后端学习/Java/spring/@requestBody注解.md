---
created: 2024-06-24
updated: 2024-06-24
Type: knowledge
Status: ⌛️ 等待
tags:
---
## 问题：@request 注解是怎么实现将网络请求变成 bean 的？


具体参考简书上的这个文章 [@RequestBody注解原理 - 简书](https://www.jianshu.com/p/c1b8315c5a03)

### 先直接看网上的总结

请求由 DispatcherServlet 处理，找到相应的 HandlerAdapter 进行处理，RequestMappingHandlerAdapter 会处理@RequestMapping 注解的请求，设置一系列参数解析器进行解析，如果参数使用@RequestBody 注解，则使用 RequestResponseBodyMethodProcessor 进行解析，此参数解析器用 HttpMessageConverter 将 HttpMessage 封装为具体的 JavaBean 对象，json 格式的数据使用 AbstractJackson2HttpMessageConverter 进行解析，内部使用 jackson 进行 json 数据的解析

### 然后看源码
这里不能直接从@requestBody 源码看，而是要从更前面的地方开始看，从 dispatcherServlet 开始看

- [ ]  spring 启动流程




  
