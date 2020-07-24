# python中date、datetime、string的相互转换

[datetime官方文档](https://docs.python.org/3.6/library/datetime.html?highlight=datetime#module-datetime)

import datetime

import time

## 1. string转datetime

```python
today_str = '2020-4-8'
date_time = datetime.datetime.strptime(today_str,'%Y-%m-%d')
```

## 2. datetime转string

```python
date_time.strftime('%Y-%m-%d')
```

## 3. datetime转时间戳

```python
time_time = time.mktime(date_time.timetuple())
```

```python
time_time = date_time.timestamp()
```

## 4. date转时间戳

```python
today = datetime.date.today()
time.mktime(today.timetuple())
```

## 5. 时间戳转string

```python
time.strftime('%Y-%m-%d',time.localtime(time_time))
```

## 5. date转datetime

```python
date = datetime.date.today()
datetime.datetime.strptime(str(date),'%Y-%m-%d') #将date转换为str，在由str转换为datetime
datetime.datetime(2020,4,8,0,0)
```

## ---END---