# Mysql中文乱码问题

环境：windows8.1

产生原因：系统默认使用gbk编码，MySQL默认utf8编码

解决方案：在数据源的url上添加：“ ?useUnicode=true&characterEncoding=utf-8 ”

```
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/leyou?useUnicode=true&characterEncoding=utf-8
    username: root
    password: 123456
    hikari:
      maximum-pool-size: 30
      minimum-idle: 10
```

# ---END---