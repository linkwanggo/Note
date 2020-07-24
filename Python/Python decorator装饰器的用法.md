# Python decorator装饰器的用法

> 无论如何，装饰器最外层函数的第一个参数都会被传递`被装饰函数`的引用（function）。
>
> 除非你手动调用了最外层函数（那么第二层函数将被视为最外层函数）------ linkwanggo

## 1. 无参数的装饰器（核心装饰器）

```python
def admin_required(fn):
    @wraps(fn)
    async def wrapper(*args, **kwargs):
        return await fn(*args, **kwargs)
    return wrapper
```

## 2. 带有参数的装饰器

> 摘取自`sanic-jwt-extended.decorators.jwt_required`
>
> 无关内容已全部删除，只看核心部分

```python
def jwt_required(function=None, allow=None, deny=None, fresh_required=False):
    def real(fn):
        @wraps(fn)
        async def wrapper(*args, **kwargs):
            return await fn(*args, **kwargs)
        return wrapper
    if function:  # 如果被装饰函数被传递，那么返回一个核心装饰器
        return real(function)
    else:
        return real  # 如果没有function被传递，一般这种情况是因为当前装饰器又被其他函数包装
      						   # 那么fn会从上文中被传递到real函数
```

## 3. 带有参数的装饰器 装饰 带有参数的装饰器

```python
def login_required(function=None, allow=None, deny=None, fresh_required=False):
    def real(fn):
        @wraps(fn)
        # 注意这里不能再传入function, 这样就相当于三层装饰器中的@jwt_required()
        @jwt_required(allow=allow, deny=deny, fresh_required=fresh_required)
        async def wrapper(*args, **kwargs):
            result = await fn(*args, **kwargs)
            return result
        return wrapper
    return real(function) if function else real
```

## ---END---

