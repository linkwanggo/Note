# Flask如何优雅的书写返回值

## 1. 方案一

```python
@api.route("/<int:uid>", methods=["GET"])
@login_required
def get_user(uid):
    user = User.get(uid)
    return jsonify(user), 200
```

## 2. 方案二

```python
@app.route('/users/<int:id>', methods=['DELETE'])
def delete_user(id):
    method_to_delete_user_by_id(id)
    return '', http.HTTPStatus.NO_CONTENT # for python 3.x
```

## 3. 方案三

自定义返回类型, 例如返回json格式：

```python
{
	”code“: xxx,
	"msg": xxx,
	"data": xxx
}
```

由于我自己用的方案一，所以方案三没有具体实现，但和`Flask如何优雅的返回错误`这篇文章一个原理，可以参考此文章。

# ---END---

