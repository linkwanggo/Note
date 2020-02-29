# flask使用sqlalchemy初始化数据库时为什么数据库为空？

```python
def register_plugins(flask_app):
    """
    注册插件
    1.sqlalchemy
    :param flask_app:
    """
    from app.model.base import db
    db.init_app(flask_app)
    with flask_app.app_context():
        db.create_all()
```

原因：模型没有被载入内存

解决方案：在特定的模块中引入该Model

哪些特定的模块在一开始就会被载入呢：

例如route, app, settings

```python
from app.model.tag import Tag
```

