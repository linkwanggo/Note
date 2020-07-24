# Debian下安装RabbitMQ

```bash
docker run -di --name=rabbitmq -p 5671:5671 -p 5672:5672 -p 4369:4369 -p 15671:15671 -p 15672:15672 -p 25672:25672 rabbitmq:management-alpine
```

**注：`management`代表容器含有管理界面，`15672`是web管理界面端口

## --END--

