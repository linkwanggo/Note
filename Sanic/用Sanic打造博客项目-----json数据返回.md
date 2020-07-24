# 用Sanic打造博客项目-----json数据返回

sanic默认使用ujson, 以前我从未接触过ujson，于是查看了文档：

[ujson中文文档](https://mpython.readthedocs.io/zh/master/library/pythonStd/ujson.html)

发现ujson只能够转换dict类型，一些复杂的类型无法转换。

无奈只好选择使用Cpython默认的json库。

## 如何使用默认的json库

Cpython默认的json库可以提供**JSONEncoder**。

当json无法进行序列化时，将调用**JSONEncoder.default**函数尝试根据用户自定义的序列化方式进行序列化。

这就为我们序列化**对象**提供了可行性。

### 自定义一个**response.json**函数

```python
import datetime
from json import dumps, JSONEncoder as _JSONEncoder
import time
import uuid
from typing import Any, Optional

from sanic.log import logger
from sanic.response import HTTPResponse

from app.exception.exceptions import ServerError
from app.util import uuid_util


class JSONEncoder(_JSONEncoder):
    def default(self, o):
        if hasattr(o, "keys") and hasattr(o, "__getitem__"):
            return dict(o)
        if isinstance(o, datetime.datetime):  # 日期类型统一返回时间戳
            return o.timestamp()
        if isinstance(o, datetime.date):
            return time.mktime(o.timetuple())
        if isinstance(o, uuid.UUID):
            return uuid_util.uuid4_str(o)
        error = f'{o} encode error'
        logger.error(error)
        raise ServerError(error)


def json(body: Any, status: int = 200, headers: Optional[Any] = None,
         content_type: str = "application/json", **kwargs: Any) -> HTTPResponse:
    return HTTPResponse(
        dumps(body, cls=JSONEncoder, **kwargs),
        headers=headers,
        status=status,
        content_type=content_type,
    )
```

当我们需要序列化复杂对象的时候，就可以用自定义的**json**函数代替**sanic.response.json**函数。

## ---END---

