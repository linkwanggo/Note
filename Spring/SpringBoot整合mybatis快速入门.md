## 参考

1. [SpringBoot整合mybatis快速入门](https://www.jianshu.com/p/541874714907)
2. [spring boot 2使用Mybatis多表关联查询](https://blog.csdn.net/gdjlc/article/details/84837897)

3. 演示示例  本机项目 `springboot_demos -> mybatis_demos`

## 前置任务

1. 创建mysql表

```sql
CREATE TABLE IF NOT EXISTS `user` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(50) NOT NULL,
  `company_id` int(11) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
 
CREATE TABLE IF NOT EXISTS `company` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(200) NOT NULL,
   PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
 
CREATE TABLE IF NOT EXISTS `account` (
  `id` int(11) NOT NULL AUTO_INCREMENT,  
  `name` varchar(200) NOT NULL,
  `user_id` int(11) NOT NULL, 
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
 
 
INSERT INTO
    `user`
VALUES
    (1, 'aa', 1),
    (2, 'bb', 2);
 
INSERT INTO
    `company`
VALUES
    (1, 'xx公司'),
    (2, 'yy公司');
 
INSERT INTO
    `account`
VALUES
    (1, '中行', 1),
    (2, '工行', 1),
    (3, '中行', 2);
```

## 代码

### 注解方式

1. 创建实体类

```java
public class User {		
	private Integer id;
	private String name;
	private Company company;
	private List<Account> accounts;	
	//getter/setter 这里省略...
}
 
public class Company {
	private Integer id;
	private String companyName;
    	//getter/setter 这里省略...
}
 
public class Account {
	private Integer id;
	private String accountName;
	//getter/setter 这里省略...
}
```

2. 创建Mapper

AccountMapper.java

```java
public interface AccountMapper {
	/*
	 * 根据用户id查询账户信息
	 */
	@Select("SELECT * FROM `account` WHERE user_id = #{userId}")
	@Results({
		@Result(property = "accountName",  column = "name")
	})
    List<Account> getAccountByUserId(Long userId);
}
```

CompanyMapper.java

```java
public interface CompanyMapper {
	/*
	 * 根据公司id查询公司信息
	 */
	@Select("SELECT * FROM company WHERE id = #{id}")
	@Results({
		@Result(property = "companyName",  column = "name")
	})
	Company getCompanyById(Long id);
}
```

UserMapper.java

```java
public interface UserMapper {
	
	/*
	 * 一对一查询
	 * property：查询结果赋值给此实体属性
	 * column：对应数据库的表字段，做为下面@One(select方法的查询参数
	 * one：一对一的查询
	 * @One(select = 方法全路径) ：调用的方法
	 */
	@Select("SELECT * FROM user WHERE id = #{id}")
	@Results({
		@Result(property = "company", column = "company_id", one = @One(select = 
"com.example.demo.mapper.CompanyMapper.getCompanyById"))		
	})
	User getUserWithCompany(Long id);
	
	/*
	 * 一对多查询
	 * property：查询结果赋值给此实体属性
	 * column：对应数据库的表字段，可做为下面@One(select方法)的查询参数
	 * many：一对多的查询
	 * @Many(select = 方法全路径) ：调用的方法
	 */
	@Select("SELECT * FROM user WHERE id = #{id}")
    @Results({ 
    	@Result(property = "id", column = "id"),//加此行，否则id值为空
        @Result(property = "accounts", column = "id", many = @Many(select =
"com.example.demo.mapper.AccountMapper.getAccountByUserId"))
    })
    User getUserWithAccount(Long id);
	
	/*
	 * 同时用一对一、一对多查询
	 */
	@Select("SELECT * FROM user")
    @Results({
    	@Result(property = "id", column = "id"),
    	@Result(property = "company", column = "company_id", one = @One(select = 
"com.example.demo.mapper.CompanyMapper.getCompanyById")),
        @Result(property = "accounts", column = "id", many = @Many(select = 
"com.example.demo.mapper.AccountMapper.getAccountByUserId"))
    })
    List<User> getAll();	
}
```

### XML配置文件方式

```xml

<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="com.example.demo.mapper.UserMapper" >
    <resultMap id="UserMap" type="com.example.demo.entity.User">
	    <id column="id" jdbcType="INTEGER" property="id" />
	    <result property="name" column="name" jdbcType="VARCHAR" />	
	    <!--封装映射company表数据，user表与company表1对1关系，配置1对1的映射
            association:用于配置1对1的映射
		                属性property：company对象在user对象中的属性名
		                属性javaType：company属性的java对象 类型
		                属性column：user表中的外键引用company表
        -->
	    <association property="company" javaType="com.example.demo.entity.Company" column="company_id">
            <id property="id" column="companyid"></id>
            <result property="companyName" column="companyname"></result>            
        </association>
        <!--配置1对多关系映射
            property：在user里面的List<Account>的属性名            
            ofType:当前account表的java类型
            column:外键
        -->
        <collection property="accounts" ofType="com.example.demo.entity.Account" column="user_id">
            <id property="id" column="accountid"></id>
            <result property="accountName" column="accountname"></result>           
        </collection>        
	  </resultMap>
 
    <select id="getAll" resultMap="UserMap"  >
       SELECT 
       u.id,u.name,c.id companyid, c.name companyname, a.id accountid,a.name accountname 
       FROM user u 
       LEFT JOIN company c on u.company_id=c.id
       LEFT JOIN account a on u.id=a.user_id
    </select>
 
</mapper>
```

