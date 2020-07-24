# Sqlchemy如何在db.session.add()后获取实例ID

## 通过调用flush函数

下面是项目中用到的例子

```python
@classmethod
    def create(cls, data: dict):
        try:
            with db.auto_commit():
                # 新增文章
                instance = cls()
                instance.user_id = g.user.id
                now = datetime.datetime.now()
                instance.create_time = now
                instance.update_time = now
                for k, v in data.items():
                    if k != "tags" and hasattr(cls, k) and v is not None:
                        setattr(instance, k, v)
                db.session.add(instance)
                db.session.flush()  # 使用flush后能获取新增实例的id
                # 新增文章标签
                tags = data.get("tags", None)
                if tags:
                    tag_ids = tags.split(",")
                    for tag_id in tag_ids:
                        at = ArticleTag()
                        at.article_id = instance.id
                        at.tag_id = tag_id
                        at.create_time = now
                        at.update_time = now
                        db.session.add(at)
                return instance
        except IntegrityError as e:
            current_app.logger.error("%s", e)
            raise DatabaseError(msg="新增失败 当前数据已存在")
        except Exception as e:
            current_app.logger.error("%s", e)
            raise DatabaseError(msg="新增失败")
```

## ---END---

