# application.yml基本配置（持续更新）

```yml
server:
  port: 8888
spring:
  application:
    name: wx-sell
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/weixindiancan?useUnicode=true&characterEncoding=utf8&serverTimezone=GMT%2B8
    username: root
    password: 123456
  jpa:
    show-sql: true
    database: mysql
```

