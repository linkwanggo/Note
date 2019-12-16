# Windows下Tomcat控制台输出乱码问题

## 解决方案：修改conf下的logging.properties

```properties
# 解决在win中的控制台乱码问题 	
# java.util.logging.ConsoleHandler.encoding = UTF-8
```

## ---END---