# # SQLAlchemy：session何时commit，何时close？

## 1. 参考阅读：[SQLAlchemy - 官方文档](https://docs.sqlalchemy.org/en/13/orm/session_basics.html#session-faq-whentocreate)

* As a general rule, keep the lifecycle of the session separate and external from functions and objects that access and/or manipulate database data. This will greatly help with achieving a predictable and consistent transactional scope.
  一般来说，将会话的生命周期与访问和/或操作数据库数据的函数和对象分开。这将极大地帮助实现可预测和一致的事务范围。
* Make sure you have a clear notion of where transactions begin and end, and keep transactions short, meaning, they end at the series of a sequence of operations, instead of being held open indefinitely.
  确保您对事务在何处开始和结束有一个清晰的概念，并保持事务简短，即它们在一系列操作中结束，而不是无限期地保持打开状态。

**官方示例**

### 1.1 不要这样写

E.g. **don’t do this:**

```python
### this is the **wrong way to do it** ###

class ThingOne(object):
    def go(self):
        session = Session()
        try:
            session.query(FooBar).update({"x": 5})
            session.commit()
        except:
            session.rollback()
            raise

class ThingTwo(object):
    def go(self):
        session = Session()
        try:
            session.query(Widget).update({"q": 18})
            session.commit()
        except:
            session.rollback()
            raise

def run_my_program():
    ThingOne().go()
```

### 1.2 稍微好一点的写法

Keep the lifecycle of the session (and usually the transaction) **separate and external**:

```python
### this is a **better** (but not the only) way to do it ###

class ThingOne(object):
    def go(self, session):
        session.query(FooBar).update({"x": 5})

class ThingTwo(object):
    def go(self, session):
        session.query(Widget).update({"q": 18})

def run_my_program():
    session = Session()
    try:
        ThingOne().go(session)
        ThingTwo().go(session)

        session.commit()
    except:
        session.rollback()
        raise
    finally:
        session.close()
```

### 1.3 最好这样做

The most comprehensive approach, recommended for more substantial applications, will try to keep the details of session, transaction and exception management as far as possible from the details of the program doing its work. For example, we can further separate concerns using a [context manager](http://docs.python.org/3/library/contextlib.html#contextlib.contextmanager):

```python
### another way (but again *not the only way*) to do it ###

from contextlib import contextmanager

@contextmanager
def session_scope():
    """Provide a transactional scope around a series of operations."""
    session = Session()
    try:
        yield session
        session.commit()
    except:
        session.rollback()
        raise
    finally:
        session.close()


def run_my_program():
    with session_scope() as session:
        ThingOne().go(session)
        ThingTwo().go(session)
```

## ---END---

