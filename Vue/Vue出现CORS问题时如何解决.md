## Vue出现CORS问题时如何解决

## 出现的错误

```
Access to XMLHttpRequest at 'http://linkwanggo.tk/api/book/upload' from origin 'http://admin.linkwanggo.tk' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.
```

一直报这个错误，但是很确定后端已经配置了CORS。

在服务端查看日志输出，发现只有OPTIONS请求，并且成功返回了200

```
127.0.0.1 - - [12/Apr/2020 01:13:27] "OPTIONS /api/book/upload HTTP/1.0" 200 -
```

由于没有报错（前后端皆没有报错），导致问题非常不明显。

## 解决方案

当我用Postman重新测试请求时，意外的得到了返回值！

```
403 too Large
```

非常明显 就是因为上传的文件超过了nginx的限制，**但坑爹的浏览器竟然错误的识别为CORS失败！**

## 总结

当出现浏览器错误文不对题的时候，请用Postman检验接口，或许会有不一样的结果！

## ---END---



