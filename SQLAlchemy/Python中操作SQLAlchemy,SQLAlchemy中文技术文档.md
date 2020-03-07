# Python中操作SQLAlchemy,SQLAlchemy中文技术文档

## 一、ORM 框架简介

对象-关系映射（Object/Relation Mapping，简称ORM），是随着面向对象的软件开发方法发展而产生的。面向对象的开发方法是当今企业级应用开发环境中的主流开发方法，关系数据库是企业级应用环境中永久存放数据的主流数据存储系统。对象和关系数据是业务实体的两种表现形式，业务实体在内存中表现为对象，在数据库中表现为关系数据。内存中的对象之间存在关联和继承关系，而在数据库中，关系数据无法直接表达多对多关联和继承关系。因此，对象-关系映射(ORM)系统一般以中间件的形式存在，主要实现程序对象到关系数据库数据的映射。

#### 1.ORM方法论基于三个核心原则：

- 简单性：以最基本的形式建模数据。
- 传达性：数据库结构被任何人都能理解的语言文档化。
- 精确性：基于数据模型创建正确标准化了的结构。

面向对象是从软件工程基本原则(如耦合、聚合、封装)的基础上发展起来的，而关系数据库则是从数学理论发展而来的，两套理论存在显著的区别。为了解决这个不匹配的现象,对象关系映射技术应运而生。O/R中字母O起源于"对象"(Object),而R则来自于"关系"(Relational)。几乎所有的程序里面，都存在对象和关系数据库。在业务逻辑层和用户界面层中，我们是面向对象的。当对象信息发生变化的时候，我们需要把对象的信息保存在关系数据库中。

当开发一个应用程序的时候(不使用O/R Mapping),可能会写不少数据访问层的代码，用来从数据库保存，删除，读取对象信息，等等。在DAL中写了很多的方法来读取对象数据，改变状态对象等等任务。而这些代码写起来总是重复的。

如果开你最近的程序，看看DAL代码，肯定会看到很多近似的通用的模式。我们以保存对象的方法为例，传入一个对象，为SqlCommand对象添加SqlParameter，把所有属性和对象对应，设置SqlCommand的CommandText属性为存储过程，然后运行SqlCommand。对于每个对象都要重复的写这些代码。　　除此之外，还有更好的办法吗？有，引入一个O/R Mapping。实质上，一个O/R Mapping会为你生成DAL。与其自己写DAL代码，不如用O/R Mapping。用O/R Mapping保存，删除，读取对象，O/R Mapping负责生成SQL，你只需要关心对象就好。对象关系映射成功运用在不同的面向对象持久层产品中，

#### 2. 一般的ORM包括以下四部分：

- 一个对持久类对象进行CRUD操作的API；
- 一个语言或API用来规定与类和类属性相关的查询；
- 一个规定mapping metadata的工具；
- 一种技术可以让ORM的实现同事务对象一起进行dirty checking, lazy association fetching以及其他的优化操作。

•ORM：及Object-Relational Mapping，把关系数据库的表结构映射到对象上

•我们先来可能一个例子：

•如果我们从数据库查出来几条数据，需要你在python中表示出来，如果你没有接触过ORM技术，你或许会使用下面的形式来存储这个数据：



```json
[
  (1, ‘feng’),
  (2, ‘shang’),
  (3, ‘huo’),
]
```

如果你想知道表结构是什么样的，是不是就费劲了，如果你想快速的取出其中的元素，就需要听听ORM的思想了。

数据库中每次查出来的数据都用一个类表示，这个类的属性和数据库中表的字段一一对应。多条数据，就是一个list，每一行数据都是一个类来表示，如下所示：



```ruby
class User(object):
    def __init__(self, id, name):
        self.id = id
        self.name = name

[
    User(1, “feng”),
    User(2, “shang”),
    User(3, “huo”),
]
```

当我们需要获得id，或者name的时候，只需要通过循环获取到对象，直接通过user1.id或者user1.name就可以获取到id和name的属性。并且使得数据的存取非常的规范，这样ORM架构应用而生。

## 二、SQLAlchemy介绍

Python中最有名的ORM架构就是SQLAlchemy，我们主要就是来学习SQLAlchemy的使用

#### 2.1 安装环境：



```undefined
pip install SQLAlchemy
```

#### 2.2 安装mysql



```css
yum install mysql-server mysql
service mysqld restart
sysctmctl restart mysql.service
```

#### 2.3 创建数据库



```undefined
create database sqlalchemy;
```

#### 2.4 授权



```bash
GRANT ALL PRIVILEGES ON *.* TO 'fxq'@'%' IDENTIFIED BY ‘123456’;
```

#### 2.5 初始化连接



```python
from sqlalchemy import create_engine
engine = create_engine('mysql://fxq:123456@192.168.100.101/sqlalchemy', echo=True)
```

echo参数为True时，会显示每条执行的SQL语句，可以关闭，
 create_engine()返回一个Engine的实例，并且它表示通过数据库语法处理细节的核心接口，在这种情况下，数据库语法将会被解释成python的类方法。
 解释说明：
 [mysql://fxq:123456@192.168.100.101/sqlalchemy](mysql://fxq:123456@192.168.100.101/sqlalchemy)
 mysql:  指定是哪种数据库连接
 第一个fxq： 用户名
 123456： fxq用户对应的密码
 192.168.100.101： 数据库的ip
 sqlalchemy： 数据库需要连接库的名字

#### 2.6 创建表格

1. 主要是通过sql语句来创建表格：



```python
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

sql = '''create table student(
    id int not null primary key,
    name varchar(50),
    age int,
    address varchar(100));
'''

engine = create_engine('mysql+pymysql://fxq:123456@192.168.100.101/sqlalchemy')
conn = engine.connect()
conn.execute(sql)
engine.connect() #表示获取到数据库连接。类似我们在MySQLdb中游标course的作用。
```

### 2. 通过ORM方式创建表格



```ruby
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time    : 2018/5/10 21:58
# @Author  : Feng Xiaoqing
# @File    : demo2.py
# @Function: -----------

from sqlalchemy import create_engine, MetaData, Table, Column, Integer, String

engine = create_engine('mysql+pymysql://fxq:123456@192.168.100.101/sqlalchemy')
metadata = MetaData(engine)


student = Table('student', metadata,
            Column('id', Integer, primary_key=True),
            Column('name', String(50), ),
            Column('age', Integer),
            Column('address', String(10)),
)

metadata.create_all(engine)
```

以上方式都可以创建数据库表
 MetaData类主要用于保存表结构，连接字符串等数据，是一个多表共享的对象
 metadata = MetaData(engine)    #绑定一个数据源的metadata
 metadata.create_all(engine)         #是来创建表，这个操作是安全的操作，会先判断表是否存在。

### 3. Table类

构造函数：



```css
Table.__init__(self, name, metadata,*args, **kwargs)
```

name    表名
 metadata      共享的元数据
 *args Column 是列定义，详见下一节
 下面是可变参数 **kwargs 定义
 schema 此表的结构名称，默认None
 autoload 自动从现有表中读入表结构，默认False
 autoload_with 从其他engine读取结构，默认None

include_columns 如果autoload设置为True，则此项数组中的列明将被引用，没有写的列明将被忽略，None表示所有都列明都引用，默认None
 mustexist 如果为True，表示这个表必须在其他的python应用中定义，必须是metadata的一部分，默认False
 useexisting 如果为True，表示这个表必须被其他应用定义过，将忽略结构定义，默认False
 owner 表所有者，用于Orcal，默认None
 quote 设置为True，如果表明是SQL关键字，将强制转义，默认False
 quote_schema  设置为True，如果列明是SQL关键字，将强制转义，默认False
 mysql_engine  mysql专用，可以设置'InnoDB'或'MyISAM'

### 4. Column类

构造函数：



```css
Column.__init__(self,  name,  type_,  *args,  **kwargs)
```

1、name 列名
 2、type_ 类型，更多类型 sqlalchemy.types
 3、*args Constraint（约束）,  ForeignKey（外键）,  ColumnDefault（默认）, Sequenceobjects（序列）定义
 4、key 列名的别名，默认None
 下面是可变参数 **kwargs
 5、primary_key 如果为True，则是主键
 6、nullable 是否可为Null，默认是True
 7、default 默认值，默认是None
 8、index 是否是索引，默认是True
 9、unique 是否唯一键，默认是False
 10、onupdate 指定一个更新时候的值，这个操作是定义在SQLAlchemy中，不是在数据库里的，当更新一条数据时设置，大部分用于updateTime这类字段
 11、autoincrement 设置为整型自动增长，只有没有默认值，并且是Integer类型，默认是True
 12、quote 如果列明是关键字，则强制转义，默认False

### 5.创建会话：

说到数据库，就离不开Session。Session的主要目的是建立与数据库的会话，它维护你加载和关联的所有数据库对象。它是数据库查询（Query）的一个入口。
 在Sqlalchemy中，数据库的查询操作是通过Query对象来实现的。而Session提供了创建Query对象的接口。
 Query对象返回的结果是一组同一映射（Identity Map）对象组成的集合。事实上，集合中的一个对象，对应于数据库表中的一行（即一条记录）。所谓同一映射，是指每个对象有一个唯一的ID。如果两个对象（的引用）ID相同，则认为它们对应的是相同的对象。
 要完成数据库查询，就需要建立与数据库的连接。这就需要用到Engine对象。一个Engine可能是关联一个Session对象，也可能关联一个数据库表。
 当然Session最重要的功能还是实现原子操作。
 ORM通过session与数据库建立连接进行通信，如下所示：



```jsx
from sqlalchemy.orm import sessionmaker

DBSession = sessionmaker(bind=engine)
session = DBSession()
```

通过sessionmake方法创建一个Session工厂，然后在调用工厂的方法来实例化一个Session对象。

### 2.7 添加数据



```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time    : 2018/5/10 20:58
# @Author  : Feng Xiaoqing
# @File    : demo1.py
# @Function: -----------

from sqlalchemy import create_engine, Column, Integer, String
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker


engine = create_engine('mysql+pymysql://fxq:123456@192.168.100.101/sqlalchemy')
DBsession = sessionmaker(bind=engine)
session = DBsession()

Base = declarative_base()

class Student(Base):
    __tablename__ = 'student'
    id = Column(Integer, primary_key=True)
    name = Column(String(100))
    age = Column(Integer)
    address = Column(String(100))

student1 = Student(id=1001, name='ling', age=25, address="beijing")
student2 = Student(id=1002, name='molin', age=18, address="jiangxi")
student3 = Student(id=1003, name='karl', age=16, address="suzhou")

session.add_all([student1, student2, student3])
session.commit()
session.close()
```

### 2.8 查询

查询是这个里面最为复杂，最为繁琐的一个步骤。
 通过Session的query()方法创建一个查询对象。这个函数的参数数量是可变的，参数可以是任何类或者是类的描述的集合。下面来看一个例子：



```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time    : 2018/5/11 21:49
# @Author  : Feng Xiaoqing
# @File    : search.py
# @Function: -----------
from sqlalchemy import Column
from sqlalchemy import Integer
from sqlalchemy import String
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

Base = declarative_base()
class Student(Base):
    __tablename__ = 'student'
    id = Column(Integer, primary_key=True)
    name = Column(String(50))
    age = Column(Integer)
    address = Column(String(100))

engine = create_engine('mysql+pymysql://fxq:123456@192.168.100.101/sqlalchemy')
DBSession = sessionmaker(bind=engine)
session = DBSession()

my_stdent = session.query(Student).filter_by(name="fengxiaoqing2").first()
print(my_stdent)
```

此时我们看到的输出结果是这样的：



```csharp
<__main__.Student object at 0x032745F0>
```

前面我们在赋值的时候，我们可以通过实例化一个对象，然后直接映射到数据库中，那我们在查询出来的数据sqlalchemy直接给映射成一个对象了（或者是每个元素为这种对象的列表），对象和我们创建表时候的class是一致的，我们就也可以直接通过对象的属性就可以直接调用就可以了。



```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time    : 2018/5/11 21:49
# @Author  : Feng Xiaoqing
# @File    : search.py
# @Function: -----------
from sqlalchemy import Column
from sqlalchemy import Integer
from sqlalchemy import String
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

Base = declarative_base()
class Student(Base):
    __tablename__ = 'student'
    id = Column(Integer, primary_key=True)
    name = Column(String(50))
    age = Column(Integer)
    address = Column(String(100))

engine = create_engine('mysql+pymysql://fxq:123456@192.168.100.101/sqlalchemy')
DBSession = sessionmaker(bind=engine)
session = DBSession()

my_stdent = session.query(Student).filter_by(name="fengxiaoqing2").first()
print(my_stdent.id,my_stdent.name,my_stdent.age,my_stdent.address)
```

结果：



```undefined
1000311 fengxiaoqing2 182 chengde
```

### filter()  过滤表的条件



```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time    : 2018/5/11 21:49
# @Author  : Feng Xiaoqing
# @File    : search.py
# @Function: -----------
from sqlalchemy import Column
from sqlalchemy import Integer
from sqlalchemy import String
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

Base = declarative_base()
class Student(Base):
    __tablename__ = 'student'
    id = Column(Integer, primary_key=True)
    name = Column(String(50))
    age = Column(Integer)
    address = Column(String(100))

engine = create_engine('mysql+pymysql://fxq:123456@192.168.100.101/sqlalchemy')
DBSession = sessionmaker(bind=engine)
session = DBSession()


my_stdent = session.query(Student).filter(Student.name.like("%feng%"))
print(my_stdent)
```

结果：



```css
SELECT student.id AS student_id, student.name AS student_name, student.age AS student_age, student.address AS student_address 
FROM student 
WHERE student.name LIKE %s
```

根据结果，我们可以看出来
 filter_by最后的结果就是一个sql语句，我们排错的时候就可以通过这个来排查我们sql是否正确。
 以下的这些过滤操作都可以在filter函数中使用：



```csharp
equals:
query(Student).filter(Student.id == 10001)
not equals:
query(Student).filter(Student.id != 100)
LIKE:
query(Student).filter(Student.name.like(“%feng%”))

IN:
query(Student).filter(Student.name.in_(['feng', 'xiao', 'qing']))
not in
query(Student).filter(~Student.name.in_(['feng', 'xiao', 'qing']))

AND:
from sqlalchemy import and_
query(Student).filter(and_(Student.name == 'fengxiaoqing', Student.id ==10001))

或者
query(Student).filter(Student.name == 'fengxiaoqing').filter(Student.address == 'chengde')

OR:
from sqlalchemy import or_
query.filter(or_(Student.name == 'fengxiaoqing', Student.age ==18))
```

### 返回列表(List)和单项(Scalar)

all()  返回一个列表



```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time    : 2018/5/11 21:49
# @Author  : Feng Xiaoqing
# @File    : search.py
# @Function: -----------
from sqlalchemy import Column
from sqlalchemy import Integer
from sqlalchemy import String
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

Base = declarative_base()
class Student(Base):
    __tablename__ = 'student'
    id = Column(Integer, primary_key=True)
    name = Column(String(50))
    age = Column(Integer)
    address = Column(String(100))

engine = create_engine('mysql+pymysql://fxq:123456@192.168.100.101/sqlalchemy')
DBSession = sessionmaker(bind=engine)
session = DBSession()

my_stdent = session.query(Student).filter(Student.name.like("%feng%")).all()
print(my_stdent)
```

结果：



```json
[<__main__.Student object at 0x031405B0>, <__main__.Student object at 0x030FCA70>, <__main__.Student object at 0x031405F0>]
```

可以通过遍历列表来获取每个对象。
 one()    返回且仅返回一个查询结果。当结果的数量不足一个或者多于一个时会报错。
 把上面的all改成one就报错了。
 first()    返回至多一个结果，而且以单项形式，而不是只有一个元素的tuple形式返回这个结果.



```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time    : 2018/5/11 21:49
# @Author  : Feng Xiaoqing
# @File    : search.py
# @Function: -----------
from sqlalchemy import Column
from sqlalchemy import Integer
from sqlalchemy import String
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

Base = declarative_base()
class Student(Base):
    __tablename__ = 'student'
    id = Column(Integer, primary_key=True)
    name = Column(String(50))
    age = Column(Integer)
    address = Column(String(100))

engine = create_engine('mysql+pymysql://fxq:123456@192.168.100.101/sqlalchemy')
DBSession = sessionmaker(bind=engine)
session = DBSession()

my_stdent = session.query(Student).filter(Student.name.like("%feng%")).first()
print(my_stdent)
```

结果：



```csharp
<__main__.Student object at 0x030A3610>
```

### filter()和filter_by()的区别：

Filter：  可以像写 sql 的 where 条件那样写 > < 等条件，但引用列名时，需要通过 类名.属性名 的方式。
 filter_by：  可以使用 python 的正常参数传递方法传递条件，指定列名时，不需要额外指定类名。，参数名对应名类中的属性名，但似乎不能使用 > < 等条件。

当使用filter的时候条件之间是使用“=="，fitler_by使用的是"="。



```objectivec
user1 = session.query(User).filter_by(id=1).first()
user1 = session.query(User).filter(User.id==1).first()
```

filter不支持组合查询，只能连续调用filter来变相实现。
 而filter_by的参数是**kwargs，直接支持组合查询。
 比如：



```swift
q = sess.query(IS).filter(IS.node == node and IS.password == password).all()
```

#### 2.9 更新

更新就是查出来，直接更改就可以了



```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time    : 2018/5/11 22:27
# @Author  : Feng Xiaoqing
# @File    : update.py
# @Function: -----------

from sqlalchemy import Column
from sqlalchemy import Integer
from sqlalchemy import String
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

Base = declarative_base()
class Student(Base):
    __tablename__ = 'student'
    id = Column(Integer, primary_key=True)
    name = Column(String(50))
    age = Column(Integer)
    address = Column(String(100))

engine = create_engine('mysql+pymysql://fxq:123456@192.168.100.101/sqlalchemy')
DBSession = sessionmaker(bind=engine)
session = DBSession()

my_stdent = session.query(Student).filter(Student.id == 1002).first()
my_stdent.name = "fengxiaoqing"
my_stdent.address = "chengde"
session.commit()
student1 = session.query(Student).filter(Student.id == 1002).first()
print(student1.name, student1.address)
```

结果：



```ruby
MariaDB [sqlalchemy]> select * from student;
+---------+---------------+------+---------+
| id      | name          | age  | address |
+---------+---------------+------+---------+
|    1002 | molin         |   18 | jiangxi |
|    1003 | karl          |   16 | suzhou  |
|  100011 | fengxiaoqing  |   18 | chengde |
|  100021 | fengxiaqing1  |  181 | chengde |
| 1000111 | fengxiaoqing  |   18 | chengde |
| 1000211 | fengxiaqing1  |  181 | chengde |
| 1000311 | fengxiaoqing2 |  182 | chengde |
+---------+---------------+------+---------+
7 rows in set (0.00 sec)

MariaDB [sqlalchemy]> select * from student;
+---------+---------------+------+---------+
| id      | name          | age  | address |
+---------+---------------+------+---------+
|    1002 | fengxiaoqing  |   18 | chengde |
|    1003 | karl          |   16 | suzhou  |
|  100011 | fengxiaoqing  |   18 | chengde |
|  100021 | fengxiaqing1  |  181 | chengde |
| 1000111 | fengxiaoqing  |   18 | chengde |
| 1000211 | fengxiaqing1  |  181 | chengde |
| 1000311 | fengxiaoqing2 |  182 | chengde |
+---------+---------------+------+---------+
7 rows in set (0.00 sec)

MariaDB [sqlalchemy]> 
```

#### 2.10 删除

删除其实也是跟查询相关的，直接查出来，调用delete()方法直接就可以删除掉。



```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time    : 2018/5/11 22:27
# @Author  : Feng Xiaoqing
# @File    : update.py
# @Function: -----------

from sqlalchemy import Column
from sqlalchemy import Integer
from sqlalchemy import String
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

Base = declarative_base()
class Student(Base):
    __tablename__ = 'student'
    id = Column(Integer, primary_key=True)
    name = Column(String(50))
    age = Column(Integer)
    address = Column(String(100))

engine = create_engine('mysql+pymysql://fxq:123456@192.168.100.101/sqlalchemy')
DBSession = sessionmaker(bind=engine)
session = DBSession()


session.query(Student).filter(Student.id == 1001).delete()
session.commit()
session.close()
```

#### 2.11 统计、分组、排序

\#######统计count()



```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time    : 2018/5/11 22:27
# @Author  : Feng Xiaoqing
# @File    : update.py
# @Function: -----------

from sqlalchemy import Column
from sqlalchemy import Integer
from sqlalchemy import String
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

Base = declarative_base()
class Student(Base):
    __tablename__ = 'student'
    id = Column(Integer, primary_key=True)
    name = Column(String(50))
    age = Column(Integer)
    address = Column(String(100))

engine = create_engine('mysql+pymysql://fxq:123456@192.168.100.101/sqlalchemy')
DBSession = sessionmaker(bind=engine)
session = DBSession()

print(session.query(Student).filter(Student.name.like("%feng%")).count())
```

###### 分组 group_by()



```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time    : 2018/5/11 22:27
# @Author  : Feng Xiaoqing
# @File    : update.py
# @Function: -----------

from sqlalchemy import Column
from sqlalchemy import Integer
from sqlalchemy import String
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

Base = declarative_base()
class Student(Base):
    __tablename__ = 'student'
    id = Column(Integer, primary_key=True)
    name = Column(String(50))
    age = Column(Integer)
    address = Column(String(100))

engine = create_engine('mysql+pymysql://fxq:123456@192.168.100.101/sqlalchemy')
DBSession = sessionmaker(bind=engine)
session = DBSession()

std_group_by = session.query(Student).group_by(Student.age)
print(std_group_by)
```

结果的sql语句如下：



```css
SELECT student.id AS student_id, student.name AS student_name, student.age AS student_age, student.address AS student_address 
FROM student GROUP BY student.age
```

###### 排序 order_by()     反序在order_by里面用desc()方法



```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time    : 2018/5/11 22:27
# @Author  : Feng Xiaoqing
# @File    : update.py
# @Function: -----------

from sqlalchemy import Column
from sqlalchemy import Integer
from sqlalchemy import String
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

Base = declarative_base()
class Student(Base):
    __tablename__ = 'student'
    id = Column(Integer, primary_key=True)
    name = Column(String(50))
    age = Column(Integer)
    address = Column(String(100))

engine = create_engine('mysql+pymysql://fxq:123456@192.168.100.101/sqlalchemy')
DBSession = sessionmaker(bind=engine)
session = DBSession()

std_ord_desc = session.query(Student).filter(Student.name.like("%feng%")).order_by(Student.id.desc()).all()
for i in std_ord_desc:
  print(i.id)
```

###### 结果：



```undefined
1000311
1000211
1000111
100021
100011
1002
```



作者：俊采星驰_87e0
链接：https://www.jianshu.com/p/0ad18fdd7eed
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。