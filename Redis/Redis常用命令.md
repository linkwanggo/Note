# Redis常用命令

- 命令是不区分大小写的，但是这里为了方便和后面的 key value 进行区分所以我全部写大写，你也可以用小写。

  - 但是需要注意的是：key 是完全区分大小写的，比如 key=codeBlog 和 key=codeblog 是两个键值

- 官网命令列表：<http://redis.io/commands>

- `SET key value`，设值。eg：`SET myblog www.youmeek.com`

- `GET key`，取值

- `SELECT 0`，切换数据库

- `INCR key`，递增数字

- `DECR key`，递减数字

- `KEYS *`，查看当前数据库下所有的 key

- `APPEND key value`，给尾部追加内容，如果要追加的 key 不存在，则相当于 SET key value

- `STRLEN key`，返回键值的长度，如果键不存在则返回 0

- `MSET key1 value1 key2 value2`，同时设置多值

- `MGET key1 value1 key2 value2`，同时取多值

- `EXPIRE key 27`，设置指定键的生存时间，27 的单位是秒

- ```
  TTL key
  ```

  ，查看键的剩余生存时间

  - 返回 -2，表示不存在，过了生存时间后被删除
  - 返回 -1，表示没有生存时间，永久存储
  - 返回正整数，表示还剩下对应的生存时间

- `PERSIST key`，清除生成时间，重新变成永久存储（重新设置 key 的值也可以起到清除生存时间的效果）

- `FLUSHDB`，清空当前数据库所有键值

- `FLUSHALL`，清空所有数据库的所有键值