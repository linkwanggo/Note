# 用Sanic打造博客项目-----异常处理

[Sanic exceptions]: https://sanic.readthedocs.io/en/latest/sanic/exceptions.html	"Sanic exceptions"

原始的sanic抛出异常时，并不会返回json格式。

因此，有必要对异常处理进行改造，使其支持对自定义异常的捕获并返回json格式的错误信息

**阅读sanic的文档发现**

> In some cases, you might want to add some more error handling functionality to what is provided by default. In that case, you can subclass Sanic’s default error handler as such:

```python
from sanic import Sanic
from sanic.handlers import ErrorHandler

class CustomErrorHandler(ErrorHandler):
    def default(self, request, exception):
        ''' handles errors that have no error handlers assigned '''
        # You custom error handling logic...
        return super().default(request, exception)

app = Sanic()
app.error_handler = CustomErrorHandler()
```

我们可以根据sanic提供的自定义异常处理的方式，定制自己的异常信息。

## 自定义异常（APIException）

```python
class ExceptionCode(Enum):
    # 资源未找到
    NOT_FOUND = 1001
    # 参数异常
    INVALID_PARAMETER = 1002
    # 认证异常
    AUTH_FAILED = 1003
    # 拒绝操作
    FORBIDDEN = 1004
    # 权限拒绝
    PERMISSION_DENIED = 1005
    # 上传类型不允许
    UPLOAD_NOT_ALLOWED = 1006
    # 电子书异常
    EPUB_ERROR = 1007
    # 方法未实现异常
    UNREALIZED_ERROR = 1008
    # 数据库操作异常
    DATABASE_ERROR = 1009

class APIException(Exception):
    code = 500
    msg = 'sorry we make a mistake ~'
    error_code = 999

    def __init__(self, msg=None, code=None, error_code=None):
        if msg:
            self.msg = msg
        if code:
            self.code = code
        if error_code:
            self.error_code = error_code
        super().__init__(msg)
        
class NotFound(APIException):
    code = 404
    msg = "resource not found"
    error_code = ExceptionCode.NOT_FOUND.value
```

## 自定义异常处理（CustomErrorHandler）	

```python
class CustomErrorHandler(ErrorHandler):
    def default(self, request, exception):
        if isinstance(exception, APIException):
            logger.error(exception)
            body = dict(msg=exception.msg, error_code=exception.error_code)
            return json(body, body.get('error_code'))
        if isinstance(exception, SanicException):
            logger.error(exception)
            body = dict(msg=exception.args[0], error_code=exception.status_code)
            return json(body, body.get('error_code'))
        if isinstance(exception, Exception):
            logger.error(exception)
            body = dict(msg='system error', error_code=500)
            return json(body, body.get('error_code'))
        return super().default(request, exception)
```

## 注册到sanic app

```python
def register_handlers(sanic_app: Sanic) -> None:
    sanic_app.error_handler = CustomErrorHandler()
```

## 

## 结果展示：

![image-20200424203459139](https://tva1.sinaimg.cn/large/007S8ZIlly1ge54wshg3aj30au05aq36.jpg)

## ---END---