---
created: 2024-05-20
updated: 2024-05-20
Type: knowledge
Status: ⌛️ 等待
tags:
---
##  报错
'getBaseMapper ()' in 'com. baomidou. mybatisplus. extension. service. impl. ServiceImpl' clashes with 'getBaseMapper ()' in 'com. baomidou. mybatisplus. extension. service. IService'; attempting to use incompatible return type

这个原因就是 mapper 类里面可能使用了实体类但是你的实现类里面却用的另一个项目里面同名的实体类

