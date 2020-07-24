# 定时备份Docker下的MySQL数据

由于mysql是单一部署，从未做过备份，想着如果哪天docker意外坏掉了，mysql不能用，那里面的数据可就完蛋了 = =。

于是就抓紧写了一份mysql的备份代码！

## 代码

代码很简单，不复杂。

**代码执行的任务：**

- 每天备份特定的数据库信息
- 删除过期的备份数据

```python
# -*- coding:utf-8 -*-
__author__ = 'linkwanggo'
__date__ = '2020/4/22 12:21'

import schedule
import time
import os
import datetime

"""
Debian下 docker mysql 定时备份任务
"""

# mysql 配置
username = 'xxx'
password = 'xxx'
host = 'xxxx'
port = 'xxxx'

# 备份文件存放地址
backup_location = '/xxxxxxxxxx/backup/mysql'

# 过期设置
expire_backup_delete = True
expire_days = 7


def mapleleaf():
    database = 'mapleleaf'
    now = datetime.datetime.now()
    # 文件名模板
    backup_name_format = 'mapleleaf-{0}.sql'
    now_str = now.strftime('%Y-%m-%d-%H:%M')
    backup_name = backup_name_format.format(now_str)
    # 备份命令
    backup_command = 'docker exec -it mysql mysqldump -h{host} -P{port} -u{username} -p{password} -B {database}' \
                     ' > {backup_location}/{backup_name}'.format(host=host,
                                                                 port=port,
                                                                 username=username,
                                                                 password=password,
                                                                 database=database,
                                                                 backup_location=backup_location,
                                                                 backup_name=backup_name)
    os.system(backup_command)
    print('mapleleaf backup over!')

    # 删除过期备份
    if expire_backup_delete:
        delete_expire_backup_command_format = "find {backup_location}/ -type f " \
                                              "-mtime +{expire_days} -name '{database}*.sql' | xargs rm -rf"
        delete_command = delete_expire_backup_command_format.format(backup_location=backup_location,
                                                                    expire_days=expire_days,
                                                                    database=database)
        os.system(delete_command)
        print('mapleleaf delete expire backup over!')


def run():
    schedule.every().day.do(mapleleaf)

    while True:
        schedule.run_pending()
        time.sleep(1)


if __name__ == '__main__':
    run()
    # mapleleaf()

```

## ---END---

