# Flask如何优雅的处理异常

## 1. 设计异常状态码

每个web程序都有自己内部定义的异常状态码，这样便于排查异常。

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
```

## 2. 定义专门处理API异常的类

`APIException`继承了flask中的HTTPException。

目的是为了在HTTPException原有功能的基础上构建出能够返回特定json格式的异常

使得异常返回结果能够统一，方便前端处理。

格式如下：

```json
{
    "error_code": 1001,
    "msg": "resource not found",
    "request": "GET /api/user/1"
}
```



```python
class APIException(HTTPException):
    code = 500
    msg = "sorry we make a mistake ~"
    error_code = 999

    def __init__(self, code=None, msg=None, error_code=None):
        if code:
            self.code = code
        if msg:
            self.msg = msg
        if error_code:
            self.error_code = error_code
        super(APIException, self).__init__(msg, None)

    def get_body(self, environ=None):
        """重写get_body方法，返回JSON格式"""
        body = dict(
            msg=self.msg,
            error_code=self.error_code,
            request="{0} {1}".format(request.method, self.get_url_no_param())
        )
        return json.dumps(body)

    def get_headers(self, environ=None):
        """重写get_headers方法，告诉浏览器返回JSON格式的数据"""
        return [("Content-Type", "application/json; charset=utf-8")]

    @staticmethod
    def get_url_no_param():
        """获取url中的去参数路径"""
        full_path = request.full_path
        main_path = full_path.split("?")[0]
        return main_path
```

## 3. 定义全局异常拦截器

当我们在特定情境下抛出APIException及其子类时，需要有一个全局异常拦截器能够拦截到APIException，并将异常信息封装为json格式再通过服务器返回到客户端。

```python
@app.errorhandler(Exception)
def exception_handler(e):
    if isinstance(e, APIException):
        return e
    elif isinstance(e, HTTPException):
        code = e.code
        msg = e.description
        error_code = ExceptionCode.NOT_FOUND.value
        return APIException(code, msg, error_code)
    else:
        # 调试模式
        if app.config["DEBUG"]:
            # 打印log TODO:
            raise e
        else:
            app.logger.error("%s", e)
            return ServerError()
```

# ---END---

