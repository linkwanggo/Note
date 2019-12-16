# MySQL多表关联查询案例

## 1. 通过分类ID查询品牌列表

> 案例1： leyou项目中通过分类ID查询品牌列表
>
> 表结构如下图：分类与品牌是多对多的关系

![](https://i.loli.net/2019/11/14/AisMRzcaZYmNwbV.png)

> 查询代码(SQL)：

```mssql
SELECT b.* FROM tb_brand b
INNER JOIN tb_category_brand cb
ON b.id = cb.brand_id
WHERE cb.category_id = 105
```

> 查询代码（Mybatis）

```java
@Select("SELECT b.* FROM tb_brand b\n" +
            "INNER JOIN tb_category_brand cb\n" +
            "ON b.id = cb.brand_id\n" +
            "WHERE cb.category_id = #{cid}")
    List<Brand> queryBrandByCategoryId(@Param("cid") Long cid);
```

......