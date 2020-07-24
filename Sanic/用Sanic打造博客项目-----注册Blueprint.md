# 用Sanic打造博客项目-----注册Blueprint

sanic是一个类flask的框架，在很多地方都提供了相同的api。

但sanic相比flask而言有一个明显的优势：**异步框架**

接下来聊一聊如何注册蓝图。

## 注册Blueprint

由于项目是向前端提供api，因此我希望所有请求的前缀都带有**/api**

通过阅读源码发现，使用**BlueprintGroup**可以实现

```python
api = Blueprint('tag', url_prefix='/tag')
```

```python
def create_blueprints_group() -> BlueprintGroup:
    blueprints = (
        tag.api
    )
    group = Blueprint.group(blueprints, url_prefix='/api')
    return group
```

```python
def register_blueprints(sanic_app: Sanic) -> None:
    group = create_blueprints_group()
    sanic_app.blueprint(group)
```

**这样，blueprint就成功注册到了sanic app中**

## 结果展示：

![image-20200424201419357](https://tva1.sinaimg.cn/large/007S8ZIlly1ge54be1bmtj30bm04974e.jpg)

## ---END---

