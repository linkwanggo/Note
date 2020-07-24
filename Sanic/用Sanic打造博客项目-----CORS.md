# 用Sanic打造博客项目-----CORS

注册cors和flask中是一样的.

```python
def register_cors(sanic_app: Sanic) -> None:
    """
    :param origins:
          The origin, or list of origins to allow requests from.
          The origin(s) may be regular expressions, case-sensitive strings,
          or else an asterisk

          Default : '*'
      :type origins: list, string or regex
    """
    CORS(sanic_app, resources={r"/api/*": {"origins": "*"}})
```

## ---END---

