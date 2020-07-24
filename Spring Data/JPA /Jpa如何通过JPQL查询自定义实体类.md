

[参考链接：湖上湖回答](https://www.imooc.com/wenda/detail/594403)

## 问题描述

前提条件

| 表名     | 字段            | 注释   |
| -------- | --------------- | ------ |
| Article  | 外键Category_id | 文章表 |
| Category |                 | 分类表 |

问题：如何通过分类对文章进行归档？

希望得到的效果：

| 分类名称 | 文章数量 |
| -------- | -------- |
| CSS      | 9篇      |
| HTML     | 1篇      |

## 解决方案

**通过自定义实体类，使用JPQL查询完成**

自定义实体类`ArchivesByCategory`

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
public class ArchivesByCategory {
    // 分类ID
    private String categoryId;
    // 分类名称
    private String categoryName;
    // 文章数量
    private Long articleCount;
}
```

dao中使用JPQL定义查询条件

```java
    /**
     * 通过分类对文章进行归档
     */
    @Query("select new com.linkwanggo.spunk.pojo.ArchivesByCategory(c.id, c.name, count(a.id)) from Article a, Category c where a.categoryId = c.id group by a.categoryId order by c.createTime desc")
    List<ArchivesByCategory> archivesByCategory();
```

## 注意

确保提供到bean类的标准路径，包括包名。例如，如果bean类被调用MyBean并且在包中com.path.to，则指向bean的标准路径将为com.path.to.MyBean。仅提供MyBean将不起作用（除非bean类在默认程序包中）。

确保使用new关键字调用bean类的构造函数。SELECT new com.path.to.MyBean(...)会工作，而SELECT com.path.to.MyBean(...)不会。

确保以与bean构造函数中期望的顺序完全相同的顺序传递属性。尝试以不同顺序传递属性将导致异常。

确保查询是有效的JPA查询，也就是说，它不是本机查询。@Query("SELECT ...")，或@Query(value = "SELECT ...")或@Query(value = "SELECT ...", nativeQuery = false)将起作用，而@Query(value = "SELECT ...", nativeQuery = true)将不起作用。这是因为本机查询无需修改就传递给JPA提供程序，并且会像这样针对基础RDBMS执行。由于new和com.path.to.MyBean是无效的SQL关键字，因此RDBMS会引发异常。

本机查询解决方案

如上所述，该new ...语法是JPA支持的机制，并且可与所有JPA提供程序一起使用。但是，如果查询本身不是JPA查询，即它是本机查询，则new ...语法将不起作用，因为该查询直接传递给基础RDBMS，该RDBMS不理解该new关键字，因为它不是该关键字的一部分。 SQL标准。

## 补充：通过SQL语句查询得到结果的方案(未尝试)

步骤1：声明投影界面

```java
package com.path.to;



public interface SurveyAnswerStatistics {

 String getAnswer();



 int getCnt();

}
```

步骤2：从查询中返回投影的属性

```java
public interface SurveyRepository extends CrudRepository<Survey, Long> {

  @Query(nativeQuery = true, value =

​      "SELECT " +

​      "  v.answer AS answer, COUNT(v) AS cnt " +

​      "FROM " +

​      "  Survey v " +

​      "GROUP BY " +

​      "  v.answer")

  List<SurveyAnswerStatistics> findSurveyCount();

}
```

使用SQL AS关键字将结果字段映射到投影属性，以进行明确的映射。