# Logback日志的标准配置

```xml
<?xml version="1.0" encoding="UTF-8"?>

<configuration>
    <!--定义日志文件的存储地址,使用绝对路径-->
    <property name="LOG_HOME" value="/Users/linkwanggo/Data/Logging/sell"/>
    <!-- 定义日志文件名前缀 -->
    <property name="LOG_INFO_NAME" value="info"/>
    <property name="LOG_ERROR_NAME" value="error"/>

    <!-- Console 输出设置 -->
    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <!--格式化输出：%d表示日期，%thread表示线程名，%-5level：级别从左显示5个字符宽度%msg：日志消息，%n是换行符-->
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
            <charset>utf8</charset>
        </encoder>
    </appender>

    <!-- 按照每天生成日志文件 -->
    <appender name="FILE_INFO" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <!-- 排除ERROR日志输出 -->
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <level>ERROR</level>
            <onMatch>DENY</onMatch>
            <onMismatch>ACCEPT</onMismatch>
        </filter>
        <!-- 滚动策略 -->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!--日志输出的文件路径-->
            <fileNamePattern>${LOG_HOME}/${LOG_INFO_NAME}.%d{yyyy-MM-dd}.log</fileNamePattern>
        </rollingPolicy>
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <appender name="FILE_ERROR" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <!-- 输出级别 ERROR及以上 -->
        <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
            <level>ERROR</level>
        </filter>
        <!-- 滚动策略 -->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!--日志输出的文件路径-->
            <fileNamePattern>${LOG_HOME}/${LOG_ERROR_NAME}.%d{yyyy-MM-dd}.log</fileNamePattern>
        </rollingPolicy>
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <!-- 异步输出 -->
    <!--<appender name="ASYNC" class="ch.qos.logback.classic.AsyncAppender">-->
        <!--&lt;!&ndash; 不丢失日志.默认的,如果队列的80%已满,则会丢弃TRACT、DEBUG、INFO级别的日志 &ndash;&gt;-->
        <!--<discardingThreshold>0</discardingThreshold>-->
        <!--&lt;!&ndash; 更改默认的队列的深度,该值会影响性能.默认值为256 &ndash;&gt;-->
        <!--<queueSize>512</queueSize>-->
        <!--&lt;!&ndash; 添加附加的appender,最多只能添加一个 &ndash;&gt;-->
        <!--<appender-ref ref="FILE"/>-->
    <!--</appender>-->

    <!--<logger name="org.apache.ibatis.cache.decorators.LoggingCache" level="DEBUG" additivity="false">-->
        <!--<appender-ref ref="CONSOLE"/>-->
    <!--</logger>-->
    <!--<logger name="org.springframework.boot" level="INFO"/>-->

    <root level="info">
        <!--<appender-ref ref="ASYNC"/>-->
        <appender-ref ref="CONSOLE"/>
        <appender-ref ref="FILE_INFO"/>
        <appender-ref ref="FILE_ERROR"/>
    </root>

</configuration>
```

