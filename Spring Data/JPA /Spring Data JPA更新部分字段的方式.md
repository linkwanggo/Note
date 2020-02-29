# Spring Data JPA更新部分字段的方式

JPA更新字段的手段有两种，一种是通过设置主键进行save()保存，一种是通过@Query注解。

## 使用save()

* 通过ID查询实体类
* 判断并修改需要更新的字段

## 使用@Query

使用@Query就是手写SQL, 可以做到按需更新