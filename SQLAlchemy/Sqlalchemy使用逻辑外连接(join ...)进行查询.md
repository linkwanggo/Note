# Sqlalchemy使用逻辑外连接(join ...)进行查询

在使用sqlalchemy之前，我们先谈谈什么是**左连接、右连接、内连接、全连接**

## 1. 左连接、右连接、内连接、全连接

### 1.1 左连接

left join 是left outer join的简写，它的全称是左外连接，是外连接中的一种。

左(外)连接，左表(a_table)的记录将会全部表示出来，而右表(b_table)只会显示符合搜索条件的记录。右表记录不足的地方均为NULL。

![](https://img-blog.csdn.net/20171209142610819?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcGxnMTc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 1.2 右连接

right join on / right outer join on，它的全称是右外连接。

与左(外)连接相反，右(外)连接，左表(a_table)只会显示符合搜索条件的记录，而右表(b_table)的记录将会全部表示出来。左表记录不足的地方均为NULL。

![](https://img-blog.csdn.net/20171209144056668?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcGxnMTc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 1.3 内连接

inner join on

组合两个表中的记录，返回关联字段相符的记录，也就是返回两个表的交集（阴影）部分。

![](https://img-blog.csdn.net/20171209135846780?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcGxnMTc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 1.4 全连接（mysql暂不支持）

MySQL目前不支持此种方式，可以用其他方式替代解决。

## 2. sqlalchemy中使用内连接

我们大部分情况下更多的会使用内连接，因此以内连接进行举例

**现有表:**

- article
- article_tag
- tag

**article_tag**是**article**与**tag**的中间表(其余表无需列出)

````python
class ArticleTag(Base):
    """标签"""
    id = Column(Integer, primary_key=True)
    article_id = Column(Integer, nullable=False, comment="文章ID")
    tag_id = Column(Integer, nullable=False, comment="标签ID")

    @orm.reconstructor
    def __init__(self):
        super().__init__()
````

**那么提问：如何通过tag_id查出所有对应的article?**

对了，当然是用inner join最为方便！

**来看一看用原生sql语句应该如何写：**

```sql
SELECT article.*
FROM article INNER JOIN article_tag ON article_tag.article_id = article.id 
WHERE article_tag.tag_id = 5
```

**然后我们使用sqlalchemy**

```python
query = Article().query
query = query.join(ArticleTag, ArticleTag.article_id == Article.id)
query = query.filter(ArticleTag.tag_id == tag_id)
articles = query.all()
```

**通过对比，发现原生sql和sqlalchemy的表达形式非常相似，用sqlalchemy关联查询竟如此简单！**

## ---END---

