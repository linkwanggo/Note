# Python执行系统命令并获取返回值

## 1. 执行系统命令无返回值

```python
os.system('ps -aux')
```

## 2. 执行系统命令有返回值

```python
command = 'ps -aux'
result = os.popen(command)
result = result.read()
for line in result.splitlines():
		print(line)
```

## ---END---

