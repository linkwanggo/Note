# Spring Data JPA 条件查询&分页

## 1. 使用Example

```java
@Test
public void contextLoads() {
    User user = new User();
    user.setUsername("y");
    user.setAddress("sh");
    user.setPassword("admin");
    ExampleMatcher matcher = ExampleMatcher.matching()
            .withMatcher("username", ExampleMatcher.GenericPropertyMatchers.startsWith())//模糊查询匹配开头，即{username}%
            .withMatcher("address" ,ExampleMatcher.GenericPropertyMatchers.contains())//全部模糊查询，即%{address}%
            .withIgnorePaths("password")//忽略字段，即不管password是什么值都不加入查询条件
            .withIgnorePaths("id");  //忽略属性：是否关注。因为是基本类型，需要忽略掉
    Example<User> example = Example.of(user ,matcher);
    List<User> list = userRepository.findAll(example);
    System.out.println(list);
}
```

Example会将为null的字段自动过滤掉，不会作为筛选条件，ExampleMatch是为了支持一些稍微复杂一些的查询，比如如果有int类型的id就需要用.withIgnorePaths()忽略掉，因为Int类型默认为0，而不是Null。
如果只是简单的字符串匹配的话，可以直接用：

```java
 Example<User> example = Example.of(user);
```

**但是使用这种方式会遇到一个问题，它没有办法实现 id > startId && id < endId 这样的操作**

## 2. @Query

@Query 注解中，如果用到 ？的形式获取参数。那么它是按照方法的参数顺序来取值的。如下面的例子：

?1 对应的就是 emailAddress 参数，?2 对应的就是 name 参数。

```java
/**
     * 待回答问答
     * @param labelId
     * @param pageable
     * @return
     */
    @Query(value = "select * from tb_problem, tb_pl where id = problemid and labelid = ? and reply = 0 order by createtime desc", nativeQuery = true)
    Page<Problem> waitList(String labelId, Pageable pageable);
```

如果有两条 SQL 语句，那么两条 SQL 会分别按顺序取参数。

nativeQuery = true 代表的本地查询。countQuery 就是分页的总条数。

```java
/**
     * 最新回复的问题
     * @param labelId
     * @param pageable
     * @return
     */
    @Query(value = "select * from tb_problem, tb_pl where id = problemid and labelid = ? order by replytime desc",
            countQuery = "select count(*) from tb_problem, tb_pl where id = problemid and labelid = ?",
            nativeQuery = true)
    Page<Problem> newReplyList(String labelId, Pageable pageable);
```

@Query 注解还支持命名参数的形式绑定查询中的名称。这种绑定不需要严格限制参数的顺序。而是按照绑定的参数来赋值。

```java
@Query("select u from User u where u.firstname = :firstname or u.lastname = :lastname")
User findByLastnameOrFirstname(@Param("lastname") String lastname,
                                 @Param("firstname") String firstname);
```



从 Spring Data JPA 1.4 版开始，@Query 还支持 SpEL 模板表达式。

它的用法是select x from #{#entityName} x。它插入 entityName 与给定存储库关联的域类型。该entityName解决如下：如果域类型已设置的 name 属性 @Entity 的注释，它被使用。否则，使用域类型的简单类名。

```java
`@Entity``public` `class` `User {` `  ``@Id``  ``@GeneratedValue``  ``Long id;` `  ``String lastname;``}` `public` `interface` `UserRepository ``extends` `JpaRepository<User,Long> {` `  ``@Query``(``"select u from #{#entityName} u where u.lastname = ?1"``)``  ``List<User> findByLastname(String lastname);``}`
```

如果 @Entity 注解中使用了 name 熟悉。@Entity(name = "my_user")。那么 #{#entityName} 对应的就是 my_user 表。

如果 @Query 和 @Modifying 组合使用，那么将会变成修改语句。也就是说支持 update、delete 等，相当于 @Update 注解，不过一般很少有人这样用。

```java
`@Modifying``@Query``(``"update User u set u.firstname = ?1 where u.lastname = ?2"``)``int` `setFixedFirstnameFor(String firstname, String lastname);`
```

## 3. 分页

在参数中添加`Pageable`即可

```java
/**
     * 最新回复的问题
     * @param labelId
     * @param pageable
     * @return
     */
    @Query(value = "select * from tb_problem, tb_pl where id = problemid and labelid = ? order by replytime desc",
            countQuery = "select count(*) from tb_problem, tb_pl where id = problemid and labelid = ?",
            nativeQuery = true)
    Page<Problem> newReplyList(String labelId, Pageable pageable);
```

