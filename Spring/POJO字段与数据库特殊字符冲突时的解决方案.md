# POJO字段与数据库特殊字符冲突时的解决方案



```java
// 将特殊字段添加``变为普通字符串
@Column("`name`")
private String name;
```

# ---END---