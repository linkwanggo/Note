# MySQL查询字段不区分大小写吗？

[摘抄自:MySQL查询字段区不区分大小写？ innodb的事务与日志的实现方式？binlog的几种日志录入格式以及区别？](https://blog.csdn.net/qq_41946557/article/details/104011119 )

MySQL的字段不区分大小写吗？今天遇到了这个问题。

答案是：默认不区分

## `默认不区分`是什么意思？

答：当我们创建数据库时，通常选择的数据库**字符检索策略**是：**utf8_general_ci**

该策略不会区分字段的大小写。

## 不区分大小写的利弊？

我认为不区分大小写有利有弊。

在我个人的博客项目中就充分展示了**利**的一面。

**利：**

例如当我需要通过**文章标题**来检索文章时，我更希望文章标题中出现的英文不论大小写都能够检索到。

这样**utf8_general_ci**就给我带来的极大的便利。

**弊：**

例如登录用户为admin，此时填写ADMIN也能登录，这似乎不和逻辑。

并且也有许多场景确实需要严格区分大小写。

## 如何区分大小写？

### 解决方案一

使用**utf8_general_cs**，表示区分大小写，或者使用**utf8_bin**，表示二进制比较，同样也区分大小写 。

创建表时，直接设置表的collate属性为**utf8_general_cs**或者**utf8_bin**；

如果已经创建表，则直接修改字段的Collation属性为**utf8_general_cs**或者**utf8_bin**。

```sql
-- 创建表：
CREATE TABLE testt(
id INT PRIMARY KEY,
name VARCHAR(32) NOT NULL
) ENGINE = INNODB COLLATE =utf8_bin;
 
-- 修改表结构的Collation属性
ALTER TABLE TABLENAME MODIFY COLUMN COLUMNNAME VARCHAR(50) BINARY CHARACTER SET utf8 COLLATE utf8_bin DEFAULT NULL;
```

### 解决方案二

直接修改sql语句，在要查询的字段前面加上binary关键字。

```sql
-- 在每一个条件前加上binary关键字
select * from user where binary username = 'admin' and binary password = 'admin';
 
-- 将参数以binary('')包围
select * from user where username like binary('admin') and password like binary('admin');
```

## ---END---