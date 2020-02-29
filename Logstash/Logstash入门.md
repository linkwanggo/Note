# Logstash入门

## 什么是**Logstash** 

Logstash是一款轻量级的日志搜集处理框架，可以方便的把分散的、多样化的日志搜集 

起来，并进行自定义的处理，然后传输到指定的位置，比如某个服务器或者文件。 

## Logstash安装与测试 

解压，进入bin目录 

```
logstash ‐e ""
```

控制台输入字符，随后就有日志输出

stdin，表示输入流，指从键盘输入 

stdout，表示输出流，指从显示器输出 

命令行参数: 

-e 执行 

--config 或 -f 配置文件，后跟参数类型可以是一个字符串的配置或全路径文件名或全路径 

路径(如：/etc/logstash.d/，logstash会自动读取/etc/logstash.d/目录下所有*.conf 的文 

本文件，然后在自己内存里拼接成一个完整的大配置文件再去执行) 

## Mysql数据导入Elasticsearch

（1）在logstash-5.6.8安装目录下创建文件夹mysqletc （名称随意） 

（2）文件夹下创建mysql.conf （名称随意） ，内容如下： 

```json
input {
  jdbc {
	  # mysql jdbc connection string to our backup databse
	  jdbc_connection_string => "jdbc:mysql://192.168.3.2:3306/tensquare_article?useUnicode=true&characterEncoding=utf8"
	  # the user we wish to excute our statement as
	  jdbc_user => "root"
	  jdbc_password => "123456"
	  # the path to our downloaded jdbc driver  
	  jdbc_driver_library => "/Users/linkwanggo/JavaSoftwares/logstash/mysql/mysql-connector-java-5.1.46.jar"
	  # the name of the driver class for mysql
	  jdbc_driver_class => "com.mysql.jdbc.Driver"
	  jdbc_paging_enabled => "true"
	  jdbc_page_size => "50"
	  #以下对应着要执行的sql的绝对路径。
	  #statement_filepath => ""
	  statement => "select id,title,content from tb_article"
	  #定时字段 各字段含义（由左至右）分、时、天、月、年，全部为*默认含义为每分钟都更新（测试结果，不同的话请留言指出）
      schedule => "* * * * *"
  }
}

output {
  elasticsearch {
	  #ESIP地址与端口
	  hosts => "192.168.3.16" 
	  #ES索引名称（自己定义的）
	  index => "tensquare"
	  #自增ID编号
	  document_id => "%{id}"
	  document_type => "article"
  }
  stdout {
      #以JSON格式输出
      codec => json_lines
  }
}
```

执行如下命令启动

```bash
logstash -f /Users/linkwanggo/JavaSoftwares/logstash/mysql/mysql.conf
```

控制台输出

```
[2020-01-20T19:02:00,296][INFO ][logstash.inputs.jdbc     ][main] (0.001208s) SELECT * FROM (select id,title,content from tb_article) AS `t1` LIMIT 50 OFFSET 0
{"@version":"1","@timestamp":"2020-01-20T11:02:00.297Z","title":"IDEA常用快捷键","content":"你想知道什么","id":"1"}
{"@version":"1","@timestamp":"2020-01-20T11:02:00.297Z","title":"Springboot2.2.2真好用","content":"我是程序员","id":"3"}
{"@version":"1","@timestamp":"2020-01-20T11:02:00.297Z","title":"Springboot2.1.2是我最常用的版本","content":"我爱Spring全家桶","id":"2"}
{"@version":"1","@timestamp":"2020-01-20T11:02:00.297Z","title":"我新加入的","content":"我新加入的\n","id":"4"}
```

